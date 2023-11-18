# Pattern 1
toEnvP 𝑠 ∧

(∀ 𝑠1. substate 𝑠1 𝑠 ∧ toEnvP 𝑠1 ∧ toEnvNum 𝑠1 𝑠 ≥ 𝜏 ∧ A1(𝑠1) ⟶

(∃ 𝑠3. toEnvP 𝑠3 ∧ substate 𝑠1 𝑠3 ∧ substate 𝑠3 𝑠 ∧ toEnvNum 𝑠1 𝑠3 ≤ 𝜏 ∧ A2(𝑠3) ∧

(∀ 𝑠2. toEnvP 𝑠2 ∧ substate 𝑠1 𝑠2 ∧ substate 𝑠2 𝑠3 ∧ 𝑠2 ≠ 𝑠3 ⟶ A3(𝑠2))))

# Pattern 2
toEnvP 𝑠 ∧

(∀ 𝑠1 𝑠2.substate 𝑠1 𝑠2 ∧ substate 𝑠2 𝑠 ∧ toEnvP 𝑠1 ∧ toEnvP 𝑠2 ∧ toEnvNum 𝑠1 𝑠2 = 1 ∧ A1(𝑠1, 𝑠2) ⟶ A2(𝑠2))

# Pattern 3
toEnvP 𝑠 ∧ (∀ 𝑠1. substate 𝑠1 𝑠 ∧ toEnvP 𝑠1 ∧ A1(𝑠1) ⟶ A2(𝑠1))

# Pattern 4
toEnvP 𝑠 ∧

(∀ 𝑠1 𝑠2. substate 𝑠1 𝑠2 ∧ substate 𝑠2 𝑠 ∧ toEnvP 𝑠1 ∧ toEnvP 𝑠2 ∧ toEnvNum 𝑠1 𝑠2 = 1 ∧ toEnvNum 𝑠2 𝑠 ≥ 𝜏 ∧

A1(𝑠1, 𝑠2) ⟶

∃𝑠4. toEnvP 𝑠4 ∧ substate 𝑠2 𝑠4 ∧ substate 𝑠4 𝑠 ∧ toEnvNum 𝑠2 𝑠4 ≤ 𝜏 ∧ A2(𝑠4) ∧

∀𝑠3.(toEnvP 𝑠3 ∧ substate 𝑠2 𝑠3 ∧ substate 𝑠3 𝑠4 ∧ 𝑠3 ≠ 𝑠4 ⟶ A3(𝑠3)))

# Pattern 5
toEnvP 𝑠 ∧

(∀ 𝑠1 𝑠2 𝑠3. substate 𝑠1 𝑠2 ∧ substate 𝑠2 𝑠3 ∧ substate 𝑠3 𝑠 ∧ toEnvP 𝑠1 ∧ toEnvP 𝑠2 ∧ toEnvP 𝑠3 ∧

toEnvNum 𝑠1 𝑠2 = 1 ∧ toEnvNum 𝑠2 𝑠3 < 𝜏 ∧ A1(𝑠1, 𝑠2) ⟶ A2(𝑠3)

# Pattern 6
toEnvP 𝑠 ∧

(∀ 𝑠1 𝑠2 𝑠3.substate 𝑠1 𝑠2 ∧ substate 𝑠2 𝑠3 ∧ substate 𝑠3 𝑠 ∧ toEnvP 𝑠1 ∧ toEnvP 𝑠2 ∧ toEnvP 𝑠3 ∧

toEnvNum 𝑠1 𝑠2 = 1 ∧ A1(𝑠1, 𝑠2) ∧

(∀𝑠4. toEnvP 𝑠1 ∧ substate 𝑠2 𝑠4 ∧ substate 𝑠4 𝑠3 ⟶ ¬ A3(𝑠4)) ⟶

A2(𝑠3)

# Pattern 7
toEnvP 𝑠 ∧
(∀ 𝑠1 𝑠2. substate 𝑠1 𝑠2 ∧ substate 𝑠2 𝑠 ∧ toEnvP 𝑠1 ∧ toEnvP 𝑠2 ∧ toEnvNum 𝑠1 𝑠2 = 1 ∧ A1(𝑠1, 𝑠2) ⟶

(∃ 𝑠4. toEnvP 𝑠4 ∧ substate 𝑠2 𝑠4 ∧ substate 𝑠4 𝑠 ∧ toEnvNum 𝑠1 𝑠2 ≤ 𝜏 ∧ A3(𝑠4) ∧

(∀ 𝑠3. toEnvP 𝑠3 ∧ substate 𝑠2 𝑠3 ∧ substate 𝑠3 𝑠4 ∧ 𝑠3 ≠ 𝑠4 ⟶ A2(𝑠3))) ∨

(∀𝑠3. toEnvP 𝑠3 ∧ substate 𝑠𝑠2 𝑠3 ∧ substate 𝑠3 𝑠 ≤ 𝜏 ⟶ A2(𝑠3)))
