https://utokyo-icepp.github.io/qc-workbook/chsh_inequality.html#

## CHSH不等式を計算する回路を書く

- ページ: https://utokyo-icepp.github.io/qc-workbook/chsh_inequality.html#id66
- コード: [ibmq01.py](ibmq01.py)

### 実行結果
```shell
n-andos-MBP16:Qiskit n-ando$ ./ibmq01.py 
Jobs will run on ibmq_athens
Job Status: job has successfully run
[{'00': 3412, '01': 610, '10': 707, '11': 3463}, {'00': 740, '01': 3517, '10': 3231, '11': 704}, {'00': 3371, '01': 707, '10': 783, '11': 3331}, {'00': 3470, '01': 635, '10': 592, '11': 3495}]
C: [0.678466796875, -0.6474609375, 0.63623046875, 0.700439453125]
S = 2.66259765625
Yes, we are using a quantum computer!
```

### コード

```python
#!/usr/bin/env python3
# https://utokyo-icepp.github.io/qc-workbook/chsh_inequality.html#id66
import numpy as np
import matplotlib.pyplot as plt
from qiskit import QuantumCircuit, IBMQ, execute
from qiskit.providers.ibmq import least_busy
from qiskit.tools.monitor import job_monitor
from qiskit.visualization import plot_histogram



circuits = []
for ic in range(4):
    circuit = QuantumCircuit(2, name='circuit{}'.format(ic))
    circuit.h(0)
    circuit.cx(0, 1)
    circuits.append(circuit)

circuits[0].ry(-np.pi / 4., 1)
circuits[1].ry(-3. * np.pi / 4., 1)
circuits[2].ry(-np.pi / 4., 1)
circuits[3].ry(-3. * np.pi / 4., 1)

circuits[2].ry(-np.pi / 2., 0)
circuits[3].ry(-np.pi / 2., 0)

for circuit in circuits:
    circuit.measure_all()

# draw()にmatplotlibのaxesオブジェクトを渡すと、そこに描画してくれる
# 一つのノートブックセルで複数プロットしたい時などに便利
for circuit in circuits:
    ax = plt.figure().add_subplot()
    circuit.draw('mpl', ax=ax)

IBMQ.load_account()

# IBMQプロバイダ（実機へのアクセスを管理するオブジェクト）
provider = IBMQ.get_provider(hub='ibm-q', group='open', project='main')

# バックエンド（実機）のうち量子ビット数2個以上のもののリストをプロバイダから取得し、一番空いているものを選ぶ
backend_filter = lambda b: (not b.configuration().simulator) and (b.configuration().n_qubits >= 2) and b.status().operational
backend = least_busy(provider.backends(filters=backend_filter))

print('Jobs will run on', backend.name())

shots = 8192

job = execute(circuits, backend=backend, shots=shots)

job_monitor(job, interval=2)

result = job.result()

counts = []
for circuit in circuits:
    c = result.get_counts(circuit)
    counts.append(c)
    
print(counts)

for c in counts:
    ax = plt.figure().add_subplot()
    plot_histogram(c, ax=ax)

C = []
for c in counts:
    C.append((c['00'] + c['11'] - c['01'] - c['10']) / shots)
    
S = C[0] - C[1] + C[2] + C[3]

print('C:', C)
print('S =', S)
if S > 2.:
    print('Yes, we are using a quantum computer!')
    
```