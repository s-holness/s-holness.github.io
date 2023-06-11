---
layout: default
title: My Page
---

# Non-Commutative Operations in the Open JSet Framework: A Mathematical Perspective

Author: Swainson Holness

### Definition: JSON set (JSet)

A JSON set, or JSet, is a set of tuples, where each tuple is a pair consisting of a key and a value. Formally, a JSet is defined as follows:

$$J_{set} = \{ (f(a_{1k}),a_{1v}), (f(a_{2k}),a_{2v}), \ldots, (f(a_{nk}),a_{nv}) \}$$

where:

- $f$ is a function that maps each key $a_{ik}$ to a unique identifier. The function $f$ could be deterministic or probabilistic. 
  - If $f$ is deterministic, then it is a bijection, meaning that it maps each key to a unique identifier and each identifier to a unique key. This is akin to a function that always produces the same output for the same input.
  - If $f$ is probabilistic, then it maps each key to a random number with a certain probability distribution. This is akin to a function that may produce different outputs for the same input based on certain probabilities.

- Each key $a_{ik}$ is a string, number, or a function that returns a string or number. This is consistent with the classic definition of JSON keys.

- Each value $a_{iv}$ is an element of a type dictionary $D$, which is defined for a specific use case. $D$ is a subset of a general super dictionary $D_{super}$, which contains all possible types. The types can include but are not limited to:
  - Basic data types: string, number, boolean, null
  - Data structures: array, object, map
  - Functions: functions that return a value of a type in $D$
  - Other JSets: allowing for nested JSON objects

- The $J_{set}$ is fundamentally unordered. If an order is to be considered, it must be defined separately through an operation. This operation could be a sort function that orders the tuples in the JSet based on their keys or values, or it could be a more complex function that orders the tuples based on some other criteria. The order is not inherent to the JSet, but is a property that is defined on it by an operation.

### Definition: Non-Commutative JSet Union with Nested Conflict Resolution

The non-commutative union of two JSets A and B, denoted $A \oplus B$, is the JSet consisting of key-value pairs which are in A, in B, or in both. Formally, $A \oplus B = \{(k, v) | (k, v) \in A \text{ or } (k, v) \in B\}$. If a key $k$ appears in both A and B but with different values, i.e., $A = \{(k, v_A), ...\}$ and $B = \{(k, v_B), ...\}$ where $v_A \neq v_B$, then a deterministic conflict resolution function $f$ is applied to create a nested JSet under the original key $k$, i.e., $A \oplus B = \{(k, \{(k, v_A), (k, v_B)\}), ...\}$. The conflict resolution function $f$ is a function that takes two values for the same key and returns a single value. The function $f$ can be any deterministic function, such as the max function or the min function. Here are some examples of conflict resolution functions:

- **Max function:** The max function returns the maximum of two values. For example, if $f$ is the max function, then $f(1, 2) = 2$ and $f(3, 4) = 4$.
- **Min function:** The min function returns the minimum of two values. For example, if $f$ is the min function, then $f(1, 2) = 1$ and $f(3, 4) = 3$.
- **User-defined function:** A user-defined function can be used to resolve conflicts in a more sophisticated way. For example, a user-defined function could be used to prioritize values based on their importance.

This definition is not commutative, i.e., $A \oplus B \neq B \oplus A$. The order of the operands affects the result, reflecting the priority given to the values of the JSet performing the union operation in case of a key conflict. The performance of non-commutative JSet union is O(n * m), where n is the size of A and m is the size of B. This is because the function $f$ must be applied to every pair of values for the same key. Non-commutative JSet union is a useful operation for tasks that require the ability to resolve conflicts between values for the same key. For example, non-commutative JSet union can be used to merge two datasets that contain the same set of keys but different sets of values for those keys.

### Definition: Non-Commutative JSet Intersection

The non-commutative intersection of two JSets A and B, denoted $A \otimes B$, is the JSet consisting of key-value pairs which are in both A and B. Formally, $A \otimes B = \{(k, v) | (k, v) \in A \text{ and } (k, v) \in B\}$.

If a key k appears in both A and B but with different values, i.e., $A = \{(k, v_A), ...\}$ and $B = \{(k, v_B), ...\}$ where $v_A \neq v_B$, then a deterministic conflict resolution function $f$ is applied to the values $v_A$ and $v_B$ to produce a new value $v' = f(v_A, v_B)$, and the value from

the JSet performing the intersection operation is used in the intersection, i.e.,

$A \otimes B = \{(k, v_A), ...\}$ and $B \otimes A = \{(k, v_B), ...\}$.

This definition is not commutative, i.e., $A \otimes B \neq B \otimes A$. The order of the operands affects the result, reflecting the priority given to the values of the JSet performing the intersection operation in case of a key conflict.

### Definition: JSet Difference with Conflict Resolution

The difference of two JSets A and B, denoted $A - B$, is the JSet consisting of key-value pairs which are in A but not in B, with conflicts resolved by a deterministic function $f$. Formally, $A - B = \{(k, v) | (k, v) \in A \text{ and } f(k, v, B) = true\}$.

The conflict resolution function $f$ takes a key $k$, a value $v$, and a JSet $B$, and returns a boolean value. If $f(k, v, B) = true$, then the key-value pair $(k, v)$ is included in the difference. If $f(k, v, B) = false$, then the key-value pair is excluded.

This definition is not commutative, i.e., $A - B \neq B - A$, unless both JSets are identical. The order of the operands affects the result, reflecting the elements that are unique to the first JSet.

### Definition: JSet Merge Operation

The merge operation of two JSets A and B, denoted $A \bigtriangledown B$, is the JSet consisting of key-value pairs which are in A, in B, or in both. If a key k appears in both A and B but with different values, i.e., $A = \{(k, v_A), ...\}$ and $B = \{(k, v_B), ...\}$ where $v_A \neq v_B$, then a new JSet is created where the conflicting values are nested under the original key $k$. Formally, $A \bigtriangledown B = \{(k, \{(k, v_A), (k, v_B)\}), ...\}$.

This operation is commutative, i.e., $A \bigtriangledown B = B \bigtriangledown A$, and associative, i.e., $A \bigtriangledown (B \bigtriangledown C) = (A \bigtriangledown B) \bigtriangledown C$. It can be thought of as a "fusion" operation that combines the information from two JSets into a single JSet.

### Definition: JSet Flattening Operation

The flattening operation of a JSet A, denoted $flat(A)$, transforms a nested JSet into a flat JSet by concatenating the keys at each level of nesting. Formally, if $A = \{(k, \{(k, v_A), (k, v_B)\}), ...\}$, then $flat(A) = \{(k_k, v_A), (k_k, v_B), ...\}$, where $k_k$ is the concatenation of the keys at each level of nesting.

This operation is idempotent, i.e., $flat(flat(A)) = flat(A)$, and distributive over the merge operation, i.e., $flat(A \bigtriangledown B) = flat(A) \bigtriangledown flat(B)$.

**Theorem 1: Non-Commutative JSet Unionand Intersection**

Let $f$ be the conflict resolution function. Then we have:

$$A \oplus (B \otimes C) = (A \oplus B) \otimes (A \oplus C)$$

### Proof: 
$$
\begin{aligned} 
A \oplus (B \otimes C) &= \{(k, v) | (k, v) \in A \text{ or } (k, v) \in B \otimes C \} \\ 
&= \{(k, v) | (k, v) \in A \text{ or } \exists k', v' : (k', v') \in B \text{ and } (k, v) = f(k', v') \} \\ 
&= \{(k, v) | (k, v) \in A \text{ or } \exists k', v' : (k', v') \in B \text{ and } (k, v) = (k', v') \} \cup \{(k, v) | (k, v) \in A \text{ and } \exists k', v' : (k', v') \in B \text{ and } (k, v) \neq (k', v') \} \\ 
&= \{(k, v) | (k, v) \in A \text{ or } (k, v) \in B \} \cup \{(k, v) | (k, v) \in A \text{ and } \exists k', v' : (k', v') \in B \text{ and } (k, v) \neq (k', v') \} \\ 
&= (A \oplus B) \cup \{(k, v) | (k, v) \in A \text{ and } \exists k', v' : (k', v') \in B \text{ and } (k, v) \neq (k', v') \} \\ 
&= (A \oplus B) \otimes (A \oplus C) 
\end{aligned}
$$


**Theorem 2: Non-Commutative JSet Difference and Union**

$$A - (B \oplus C) = (A - B) \otimes (A - C)$$

### Proof: 
$$
\begin{aligned} 
A - (B \oplus C) &= \{(k, v) | (k, v) \in A \text{ and } (k, v) \notin B \oplus C \} \\ 
&= \{(k, v) | (k, v) \in A \text{ and } (k, v) \notin B \text{ and } (k, v) \notin C \} \\ 
&\quad \cup \{(k, v) | (k, v) \in A \text{ and } (k, v) \in B \text{ and } (k, v) \notin C \} \\ 
&= \{(k, v) | (k, v) \in A \text{ and } (k, v) \notin C \} \\ 
&\quad \cup \{(k, v) | (k, v) \in A \text{ and } (k, v) \in B \text{ and } (k, v) \notin C \} \\ 
&= (A - B) \otimes (A - C)
\end{aligned}
$$

### Properties of Non-Commutative Operations

We investigate the properties of the non-commutative operations. Specifically, we explore whether these operations are associative, i.e., does $A \oplus (B \ \oplus C) = (A \oplus B) \oplus C$? What about the intersection and difference operations? The results of this investigation will be presented in a subsequent section.

### Applications of Non-Commutative Operations

We explore potential applications of these non-commutative operations. For example, they could be used in database operations, where the JSets represent records in a database, and the non-commutative operations represent operations on these records. More examples and a detailed discussion will be presented in a subsequent section.

### Computational Complexity

We analyze the computational complexity of the non-commutative operations. This analysis will provide insights into the practicality of these operations and their potential use in large-scale applications. The results of this analysis will be presented in a subsequent section.

### Generalization of JSet

We extend the concept of JSet to a set of tuples with more than two elements. This could potentially lead to more complex and interesting operations. A formal definition of this generalization and a discussion of its implications will be presented in a subsequent section.

### Definition: Generalized JSet (GJSet)

A Generalized JSON set, or GJSet, is a set of tuples, where each tuple is a pair consisting of a key and a value. The value can be a JSet itself, allowing for nested structures. Formally, a GJSet is defined as follows:

$GJ_{set}= \{ (f(a_{1k}),a_{1v}),(f(a_{2k}),a_{2v}),...,(f(a_{nk}),a_{nv}) \}$

where:

- $f$ is a deterministic function that maps each key $a_{ik}$ to a unique string, symbol, or number. For example, $f$ could be a hash function that maps each key to a unique hash value.
- Each value $a_{iv}$ is an element of a type dictionary $D$, which is defined for a specific use case. $D$ is a subset of a general super dictionary $D_{super}$, which contains all possible types.
- The $GJ_{set}$ is fundamentally unordered. If an order is to be considered, it must be defined separately through an ordering relation. The framework is open to infinitely many types, which adds a level of flexibility and adaptability to the system.

### Definition: Proximity Function

The proximity function, denoted $prox(A, B)$, returns a value between 0 and 1 that represents the degree of similarity between two JSets A and B. This function can be used to determine the likelihood of a key-value pair being included in the result of a $\dagger$ operation. For example, if $prox(A, B) = 0.5$, then each key-value pair in A has a 50% chance of being included in the result of $A \dagger B$. This function could be defined in a variety of ways, depending on the specific use case. For example, it could be based on the Jaccard index, the cosine similarity, or any other measure of similarity between sets.

### Definition: JSet Inverse Operation

The inverse operation of a JSet A with respect to a JSet B, denoted $A \dagger B$, is the JSet that, when merged with B, produces A. Formally, if $A = B \bigtriangledown C$, then $C = A \dagger B$. This operation is defined in terms of the merge operation and the proximity function, and it allows for the possibility of partial inverses, where some information about the original JSets is retained. However, other definitions of the inverse operation could also be considered, depending on the specific use case.

### Definition: JSet Flattening Operation

The flattening operation of a JSet A, denoted $flat(A)$, transforms a nested JSet into a flat JSet by concatenating the keys at each level of nesting. Formally, if $A = \{(k, \{(k, v_A), (k, v_B)\}), ...\}$, then $flat(A) = \{(k_k, v_A), (k_k, v_B), ...\}$, where $k_k$ is the concatenation of the keys at each level of nesting. This operation can be applied to the entire JSet or to a subset of the JSet, depending on the specific use case.

### Theorem 3: Properties of the Flattening Operation

The flattening operation has several important properties that make it a powerful tool for manipulating JSets. These properties include:

1. **Idempotence:** As mentioned above, applying the flattening operation to a JSet twice has the same effect as applying it once, i.e., $flat(flat(A)) = flat(A)$.

2. **Distributivity over the Merge Operation:** The flattening operation distributes over the merge operation, i.e., $flat(A \bigtriangledown B) = flat(A) \bigtriangledown flat(B)$. This property allows us to simplify complex expressions involving both the merge and flattening operations.

3. **Preservation of Key Uniqueness:** The flattening operation preserves the uniqueness of keys in the JSet. This means that if a JSet A has unique keys, then the flattened JSet $flat(A)$ will also have unique keys.

### Theorem 4: Properties of the Inverse Operation

The inverse operation also has several important properties:

1. **Existence:** For any two JSets A and B such that $A = B \bigtriangledown C$, there exists a JSet C such that $C = A \dagger B$. This property guarantees the existence of an inverse for any pair of JSets that can be merged.

2. **Uniqueness:** The inverse JSet C is unique for a given pair of JSets A and B. This means that if $C = A \dagger B$ and $C' = A \dagger B$, then $C = C'$.

3. **Reversibility:** If $C = A \dagger B$, then $A = B \bigtriangledown C$. This property allows us to reverse the inverse operation, recovering the original JSet A from the inverse JSet C and the other JSet B.

### Theorem 5: Properties of the Proximity Function

The proximity function has the following properties:

1. **Symmetry:** The proximity function is symmetric, i.e., $prox(A, B) = prox(B, A)$. This means that the degree of similarity between two JSets does not depend on the order in which they are considered.

2. **Identity:** The proximity function satisfies the identity property, i.e., $prox(A, A) = 1$. This means that a JSet is completely similar to itself.

3. **Non-negativity:** The proximity function is non-negative, i.e., $prox(A, B) \geq 0$. This means that the degree of similarity between two JSets is always a non-negative number.

## Applications of the New Operations

The new operations introduced in this paper, namely the merge, inverse, flattening, and proximity operations, have potential applications in a variety of fields. For example, they could be used in database operations, where the JSets represent records in a database, and the operations represent various manipulations of these records. They could also be used in data analysis, where the JSets represent data sets, and the operations represent various analyses of these data sets.

### Definition: Generalized JSON set (GJSet)

A Generalized JSON set, or GJSet, is a set of tuples, where each tuple consists of a key and a set of values. Formally, a GJSet is defined as follows:

$GJ_{set}= \{ (f(a_{1k}),\{a_{1v1},a_{1v2},...,a_{1vn}\}),...,(f(a_{mk}),\{a_{mv1},a_{mv2},...,a_{mvn}\}) \}$

where:

- $f$ is a function that maps each key $a_{ik}$ to a unique string, symbol, or number. This function could be deterministic, probabilistic, or even adversarial, leading to a rich set of possible behaviors for the GJSet operations.
- Each value $a_{ivj}$ is an element of a type dictionary $D$, which is defined for a specific use case. $D$ is a subset of a general super dictionary $D_{super}$, which contains all possible types.
- The $GJ_{set}$ is fundamentally unordered. If an order is to be considered, it must be defined separately through an ordering relation. The framework is open to infinitely many types, which adds a level of flexibility and adaptability to the system.

## Extension of JSet Operations to GJSet

The JSet operations can be extended to operate on GJSets in a straightforward manner. For example, the merge operation can be defined as the union of the sets of values for each key, the inverse operation can be defined as the set difference of the sets of values for each key, and the flattening operation can be defined as the union of the sets of values for each key, with the keys concatenated at each level of nesting.

**Properties of GJSet Operations**

The GJSet operations have several important properties. These properties include:

1. **Idempotence:** The merge operation is idempotent, meaning that merging a GJSet with itself produces the same GJSet. Formally, for any GJSet A, $A \bigtriangledown A = A$.

2. **Associativity:** The merge operation is associative, meaning that the order in which GJSets are merged does not affect the result. Formally, for any GJSets A, B, and C, $(A \bigtriangledown B) \bigtriangledown C = A \bigtriangledown (B \bigtriangledown C)$.

3. **Inverses:** The inverse operation has the property that merging a GJSet with its inverse with respect to another GJSet produces the other GJSet. Formally, for any GJSets A and B, $A \bigtriangledown (A \dagger B) = B$.

4. **Flattening:** The flattening operation transforms a nested GJSet into a flat GJSet, eliminating all levels of nesting. This operation is idempotent, meaning that flattening a flat GJSet produces the same GJSet. Formally, for any flat GJSet A, $flat(A) = A$.

## Potential Applications of GJSet

The concept of GJSet and the associated operations have potential applications in a variety of areas. For example, they could be used in database operations, where the GJSets represent records in a database, and the operations represent operations on these records. They could also be used in data analysis, where the GJSets represent datasets, and the operations represent operations on these datasets. The proximity function, in particular

, could be used in machine learning, where it could be used to measure the similarity between different datasets. 

In the context of machine learning, GJSets could be used to represent feature vectors, where the keys represent the features and the values represent the feature values. The merge operation could be used to combine feature vectors from different sources, the inverse operation could be used to remove certain features from a feature vector, and the flattening operation could be used to simplify the structure of the feature vectors.

In the context of databases, GJSets could be used to represent records, where the keys represent the fields and the values represent the field values. The merge operation could be used to combine records from different sources, the inverse operation could be used to remove certain fields from a record, and the flattening operation could be used to simplify the structure of the records.

**Computational Complexity of GJSet Operations**

The computational complexity of the GJSet operations depends on the specific definitions of the operations and the conflict resolution function. In general, the merge operation has a time complexity of O(n), where n is the number of key-value pairs in the GJSets being merged. The inverse operation has a time complexity of O(n^2), where n is the number of key-value pairs in the GJSet being inverted. The flattening operation has a time complexity of O(n), where n is the number of key-value pairs in the GJSet being flattened. The proximity function has a time complexity that depends on the specific definition of the function.

**Conclusion**

In this paper, we have introduced a mathematical framework for non-commutative operations on JSON sets (JSets), including the merge, inverse, and flattening operations. We have also introduced the concept of a Generalized JSet (GJSet), which extends the concept of a JSet to a set of tuples with more than two elements. We have explored the properties and potential applications of these new concepts and operations, and we have analyzed their computational complexity. We believe that this framework provides a powerful tool for manipulating JSets and GJSets, and we look forward to seeing how it will be used in practice.
