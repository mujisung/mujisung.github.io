---
layout: posts
title: lsun datset 다운로드 받는 방법 #title
categories: new_note
tag: new_note
author_profile: false # 글을 누르면 내 소개가 없어짐. true로 하면 얼굴이 나옴.
sidebar:      # 글을 누르면 목차가 나온다.
  nav: "counts" 
search: true     # false라고 하면 글이 검색이 안된다.
---

<div class="notice--info" markdown="1" style='font-size: 20px'>
**motivation:** lsun dataset download가 시간이 너무 많이 걸리는데, 단축하는 방법
</div>

<br>

# lsun dataset 다운받는 방법

[https://github.com/fyu/lsun](https://github.com/fyu/lsun)

여기에 lsun dataset을 다운받을 수 있는 download.py가 있고, 이거를 `python3 download.py` 를 하면 자동으로 다운받아지는데, curl을 통해서 다운을 하는데, 이게 시간이 너무 많이 걸리고, 도중에 다운이 멈춰서 끝난다. 

이거를 해결하기 위해서, 파일을 30개로 나누고, 각각 다운받고 합치는 방식으로 해결하면, 다운이 잘 된다.

\+ 다운을 하다가 60초 정도 멈추게 되면 다시 그 지점부터 다시 다운을 시도하도록 하는 curl 커멘드 옵션을 주면 된다.

<br>

아래 코드를 `download_parellel.py`로 저장하고, `python3 download_parellel.py --parts 30` (parts 30인자는 안줘도 됨) 으로 실행하면 되는데, 근데 중요한 점은 


## nohup 실행 방법

나는 nohup으로 실행시켰다.

`mkdir -p download2`

`nohup python3 download_parellel.py --out_dir ./download2 --parts 30  > ./download2/download.log 2>&1 &` 

이렇게 해서 서버에서 로그아웃을 하더라도 잘 실행될 수 있게 했다.

## nohup 끄는 방법

nohup를 끄는 방법은 

ps -ef | grep "python3"

하면 

kill 3957020 3957033 3957034 3957035 3957036 3957037 3957038 3957039 3957040

kill -9 3957020 3957033 3957034 3957035 3957036 3957037 3957038 3957039 3957040


## 프로세스의 실행 시간 확인

ps -p 3957020 -o etime,lstart

이렇게 하면 실행 시간이 뜬다. 시작한 시간이랑.

ps -p 3957020 -o pid,etime,lstart,cmd




# download_parellel.py

``` python

# -*- coding: utf-8 -*-

from __future__ import print_function, division
import argparse
import os
from os.path import join
import subprocess
from urllib.request import Request, urlopen
from multiprocessing import Pool

__author__ = 'Fisher Yu'
__email__ = 'fy@cs.princeton.edu'
__license__ = 'MIT'


def list_categories():
    url = 'http://dl.yf.io/lsun/categories.txt'
    with urlopen(Request(url)) as response:
        return response.read().decode().strip().split('\n')


def download_range(url, out_path, start, end):
    headers = f'Range: bytes={start}-{end}'

        # 최대 72시간으로 설정
    max_time_seconds = 72 * 60 * 60  # 72시간 = 72 * 60분 * 60초

    cmd = [
        'curl',
        '-H', headers,
        '-C', '-',  # 이어받기
        '--retry', '10',  # 최대 10회 재시도
        '--retry-delay', '5',  # 재시도 전 5초 대기
        '--speed-limit', '5000',  # 최소 다운로드 속도 (바이트/초)
        '--speed-time', '60',  # 최소 속도 시간 (초)
        '--max-time', str(max_time_seconds),  # 최대 다운로드 시간 (초)
        '-o', out_path,
        url
    ]
    subprocess.call(cmd)


def download_part(out_dir, category, set_name, part, start, end):
    url = 'http://dl.yf.io/lsun/scenes/{category}_' \
          '{set_name}_lmdb.zip'.format(**locals())
    if set_name == 'test':
        out_name = f'test_lmdb.part{part}.zip'
        url = 'http://dl.yf.io/lsun/scenes/{set_name}_lmdb.zip'
    else:
        out_name = f'{category}_{set_name}_lmdb.part{part}.zip'.format(**locals())
    out_path = join(out_dir, out_name)
    download_range(url, out_path, start, end)


def merge_files(out_dir, category, set_name, num_parts):
    if set_name == 'test':
        merged_name = 'test_lmdb.zip'
    else:
        merged_name = f'{category}_{set_name}_lmdb.zip'
    merged_path = join(out_dir, merged_name)

    with open(merged_path, 'wb') as merged:
        for part in range(num_parts):
            if set_name == 'test':
                part_name = f'test_lmdb.part{part}.zip'
            else:
                part_name = f'{category}_{set_name}_lmdb.part{part}.zip'
            part_path = join(out_dir, part_name)
            with open(part_path, 'rb') as part_file:
                merged.write(part_file.read())
            os.remove(part_path)


def download_parallel(out_dir, category, set_name, num_parts):
    if set_name == 'test':
        url = 'http://dl.yf.io/lsun/scenes/test_lmdb.zip'
    else:
        url = 'http://dl.yf.io/lsun/scenes/{category}_{set_name}_lmdb.zip'.format(**locals())

    # 파일 크기를 미리 알아냅니다
    request = Request(url)
    request.get_method = lambda: 'HEAD'
    with urlopen(request) as response:
        file_size = int(response.headers['Content-Length'])

    part_size = file_size // num_parts
    ranges = [(i * part_size, (i + 1) * part_size - 1) for i in range(num_parts)]
    ranges[-1] = (ranges[-1][0], file_size - 1)

    pool_args = [(out_dir, category, set_name, i, start, end) for i, (start, end) in enumerate(ranges)]
    with Pool(processes=num_parts) as pool:
        pool.starmap(download_part, pool_args)

    merge_files(out_dir, category, set_name, num_parts)


def main():
    parser = argparse.ArgumentParser()
    parser.add_argument('-o', '--out_dir', default='')
    parser.add_argument('-c', '--category', default=None)
    parser.add_argument('-p', '--parts', type=int, default=30, help='Number of download parts')
    args = parser.parse_args()

    categories = list_categories()
    if args.category is None:
        print('Downloading', len(categories), 'categories')
        for category in categories:
            download_parallel(args.out_dir, category, 'train', args.parts)
            download_parallel(args.out_dir, category, 'val', args.parts)
        download_parallel(args.out_dir, '', 'test', args.parts)
    else:
        if args.category == 'test':
            download_parallel(args.out_dir, '', 'test', args.parts)
        elif args.category not in categories:
            print('Error:', args.category, "doesn't exist in", 'LSUN release')
        else:
            download_parallel(args.out_dir, args.category, 'train', args.parts)
            download_parallel(args.out_dir, args.category, 'val', args.parts)


if __name__ == '__main__':
    main()


```


# 근데 사실 위 코드로 안하고,

http://dl.yf.io/lsun/

여기 들어가서, dataset을 일일이 더 빠르게 다운받을 수 있을듯.

wget으로 그냥 다운 가능.