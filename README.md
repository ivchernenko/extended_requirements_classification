# Classification of requirements
| Requirement in natural language | Requirement in DV-TRL | Pattern |
| ------------------------------- | --------------------- | ------- |
| Сигнал open должен быть в состоянии true не более 10 с. | toEnvP s ∧ (∀ s1. substate s1 s ∧ toEnvP s1 ∧ toEnvNum s1 s ≥ 100 ∧  getVarBool s1 open' ⟶ (∃ s3. toEnvP s3 ∧ substate s1 s3 ∧ substate s3 s ∧ toEnvNum s1 s3 ≤ 100 ∧ ¬ getVarBool s3 open' ∧ (∀ s2. toEnvP s2 ∧ substate s1 s2 ∧ substate s2 s3 ∧ s2 ≠ s3 ⟶ getVarBool s2 open'))) | P1 |
