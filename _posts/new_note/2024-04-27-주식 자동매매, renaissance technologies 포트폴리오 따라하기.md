---
layout: posts
title: 주식 자동매매, renaissance technologies 포트폴리오 따라하기
categories: 주식 
tag: 주식
author_profile: false # 글을 누르면 내 소개가 없어짐. true로 하면 얼굴이 나옴.
sidebar:      # 글을 누르면 목차가 나온다.
  nav: "counts" 
search: true     # false라고 하면 글이 검색이 안된다.
---

<div class="notice--info" markdown="1" style='font-size: 20px'>
**motivation:**  주식 자동매매, renaissance technologies 포트폴리오 따라하기
</div>

여기서 검색함: 
[https://www.sec.gov/edgar/searchedgar/cik](https://www.sec.gov/edgar/searchedgar/cik)


[https://www.sec.gov/edgar/browse/?CIK=1037389](https://www.sec.gov/edgar/browse/?CIK=1037389)

우선 여기에 있는 Form type: 13F-HR의, Filling에 가보면,

.xml 파일로 다운받는다. 들어가서 ctrl + s 하면 다운 된다.

그리고 python 파일 실행.


``` python
import pandas as pd
import xml.etree.ElementTree as ET

# The path to the XML file
file_path = './renaissance13Fq42023_holding.xml'

# Parse the XML file
tree = ET.parse(file_path)
root = tree.getroot()

# Define the namespace
ns = {'ns': 'http://www.sec.gov/edgar/document/thirteenf/informationtable'}

# Define a list to store the data
data = []

# Extract information for each 'infoTable' entry in the XML file
for infoTable in root.findall('ns:infoTable', ns):
    entry = {}
    entry['nameOfIssuer'] = infoTable.find('ns:nameOfIssuer', ns).text
    entry['titleOfClass'] = infoTable.find('ns:titleOfClass', ns).text
    entry['cusip'] = infoTable.find('ns:cusip', ns).text
    entry['value'] = int(infoTable.find('ns:value', ns).text)
    shrsOrPrnAmt = infoTable.find('ns:shrsOrPrnAmt', ns)
    entry['sshPrnamt'] = int(shrsOrPrnAmt.find('ns:sshPrnamt', ns).text)
    entry['sshPrnamtType'] = shrsOrPrnAmt.find('ns:sshPrnamtType', ns).text
    entry['investmentDiscretion'] = infoTable.find('ns:investmentDiscretion', ns).text
    entry['otherManager'] = int(infoTable.find('ns:otherManager', ns).text)
    votingAuthority = infoTable.find('ns:votingAuthority', ns)
    entry['Sole'] = int(votingAuthority.find('ns:Sole', ns).text)
    entry['Shared'] = int(votingAuthority.find('ns:Shared', ns).text)
    entry['None'] = int(votingAuthority.find('ns:None', ns).text)
    
    # Append the entry to the data list
    data.append(entry)

# Create a DataFrame from the data
df = pd.DataFrame(data)

# Save the DataFrame to an Excel file
excel_file_path = './renaissance13Fq42023_holding.xlsx'
df.to_excel(excel_file_path, index=False)

# Return the path to the saved Excel file
excel_file_path


```



