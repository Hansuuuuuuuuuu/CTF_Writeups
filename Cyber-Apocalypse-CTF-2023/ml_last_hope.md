# ML 3: Last Hope
> "The quantum data came back and analyzed. DISASTER! Our best scientists all agree: Unfortunately our species and our whole culture are about to be eliminated. Due to abnormal behavior of the black hole's singularity our planet is about to get swallowed. Project "ONESHOT" is our last hope..."


#### Difficulty: medium

We are given a zip file that contains a file: `quantum_artifact.qasm`. I opened the file using `less` and Googled the line `include "qelib1.inc"`, which led me to [qiskit-terra](https://github.com/Qiskit/qiskit-terra).

After installing `qiskit`, I used the following python script to execute and print the results of the qasm file:

```
from qiskit import QuantumCircuit, Aer, execute

qc = QuantumCircuit.from_qasm_file("quantum_artifact.qasm")
backend = Aer.get_backend("qasm_simulator")

job = execute(qc, backend)
result = job.result()
print(result.get_counts())
```

Which gave me the following output:
`{'01001000010101000100001001111011011000010101111101100111011011000011000101101101011100000111001100110011010111110011000001100110010111110110100000110000011100000011001101111101': 1024}
`

I converted the binary to ASCII, which gave me the flag:

`HTB{a_gl1mps3_0f_h0p3}`