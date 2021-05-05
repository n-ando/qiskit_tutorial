https://utokyo-icepp.github.io/qc-workbook/chsh_inequality.html#

## CHSH不等式を計算する回路を書く

- ページ: https://utokyo-icepp.github.io/qc-workbook/chsh_inequality.html#id66
- コード: [ibmq01.py](ibmq01.py)

```
n-andos-MBP16:Qiskit n-ando$ ./ibmq01.py 
Jobs will run on ibmq_athens
Job Status: job has successfully run
[{'00': 3412, '01': 610, '10': 707, '11': 3463}, {'00': 740, '01': 3517, '10': 3231, '11': 704}, {'00': 3371, '01': 707, '10': 783, '11': 3331}, {'00': 3470, '01': 635, '10': 592, '11': 3495}]
C: [0.678466796875, -0.6474609375, 0.63623046875, 0.700439453125]
S = 2.66259765625
Yes, we are using a quantum computer!
```
