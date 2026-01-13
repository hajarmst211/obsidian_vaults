# First normal form (1NF):
- Using row order to convey some kind of information is not permitted (sorting the people by their heights is no, we must create a separate column for that)
- mixing data types in the same column is not okay
- Every table must have a primary key
- Storing a repeating group of data items on a single row violates the 1NF. Do this instead.
![[palyer_inventory.png|500]]
# Second normal form (2NF):
![[Pasted image 20251221183213.png|450]]
#### Conclusion:
Each attribut must depend on the entire primary key
# Third normal form (3NF):
- Transitive dependency:
-> player rating deoends on player-id
-> player_skill_level depends on the player_rating
![[Pasted image 20251221182714.png|500]]
- 3NF forbids the dependency of a non key attribut on a non key attribut.
- **Solution:**
![[Pasted image 20251221182937.png|500]]
#### Conclusion:
Every attribut on a table should depend on the key, the whole key(because sometimes the key can be two attributs), and nothing but the key 
