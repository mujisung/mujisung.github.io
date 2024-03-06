

기존의 MoeLayer 클래스에 전문가 선택 빈도를 추적하고, 이를 바탕으로 보정된 보조 손실을 계산하는 기능을 추가하는 방식으로 클래스를 재구성해 보겠습니다. 이를 위해, 각 전문가의 선택 빈도를 추적하는 메커니즘과, 이 정보를 활용하여 보조 손실을 조정하는 로직을 추가합니다.

python
Copy code
import torch
import torch.nn as nn
import torch.nn.functional as F

class MoeLayer(nn.Module):
    def __init__(self, experts: list[nn.Module], gate: nn.Module, num_experts_per_tok: int):
        super().__init__()
        assert len(experts) > 0
        self.experts = nn.ModuleList(experts)
        self.gate = gate
        self.num_experts_per_tok = num_experts_per_tok
        
        # 전문가 선택 빈도 추적
        self.expert_selection_frequency = torch.zeros(len(experts), dtype=torch.float32)
        
        self.initialize_weights()

    def forward(self, inputs: torch.Tensor):
        gate_logits = self.gate(inputs)
        weights, selected_experts = torch.topk(gate_logits, self.num_experts_per_tok)
        weights = F.softmax(weights, dim=-1)
        results = torch.zeros_like(inputs)

        for i, expert in enumerate(self.experts):
            batch_idx, nth_expert = torch.where(selected_experts == i)
            # 결과에 가중치 적용 및 업데이트
            results[batch_idx] += weights[batch_idx, nth_expert, None] * expert(inputs[batch_idx])
            # 전문가 선택 빈도 업데이트
            self.expert_selection_frequency[i] += batch_idx.size(0)

        # 보조 손실 계산
        aux_loss = self.compute_auxiliary_loss(gate_logits)

        return results, aux_loss

    def compute_auxiliary_loss(self, gate_logits):
        gate_probs = F.softmax(gate_logits, dim=-1)
        f_i = gate_probs.mean(dim=0)
        P_i = torch.mean(torch.eq(gate_probs, torch.max(gate_probs, dim=-1, keepdim=True).values).float(), dim=0)

        # 전문가 선택 빈도를 기반으로 보정된 보조 손실 계산
        # 선택 빈도가 낮은 전문가에 대한 가중치 증가
        selection_frequency_normalized = self.expert_selection_frequency / self.expert_selection_frequency.sum()
        corrected_f_i = f_i * (1 / selection_frequency_normalized.clamp(min=1e-5))
        
        aux_loss = corrected_f_i @ P_i * len(self.experts)
        return aux_loss

    def initialize_weights(self):
        for expert in self.experts:
            if isinstance(expert, nn.Linear):
                nn.init.kaiming_normal_(expert.weight, mode='fan_out')
        nn.init.kaiming_normal_(self.gate.weight, mode='fan_out')
이 클래스의 주요 변경 사항은 다음과 같습니다:

전문가 선택 빈도 추적: self.expert_selection_frequency 변수를 추가하여 각 전문가가 선택된 횟수를 추적합니다.
보정된 보조 손실 계산: compute_auxiliary_loss 메소드에서 전문가의 선택 빈도를 고려하여 보정된 보조 손실을 계산합니다. 이는 전체적으로 균등한 전문가 선택을 유도합니다.
이 코드는 모델의 전문가 선택 메커니즘에 대한 균형을 더 잘 유지하도록 설계되었습니다. 하지만