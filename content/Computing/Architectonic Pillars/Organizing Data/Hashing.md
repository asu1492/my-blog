# Hashing

**Summary**: Covers hash maps, hash sets, frequency counting, lookup optimization, and hashing-based interview patterns.
**Tags**: #dsa #hashing #interview
**Created**: Unknown
**Last Updated**: 2026-05-20

---

- **Majority Element** (or II) [LeetCode 169](https://leetcode.com/problems/majority-element/description/) / [229](https://leetcode.com/problems/majority-element-ii/description/) → Boyer-Moore voting or HashMap.
- **First Missing Positive** LeetCode 41 (Hard but asked as medium) O(n) time, O(1) space → Use array as hash, mark presence by negating index.
- **Top N Users by Word Count from Logs** Given logs with timestamp, username, message. Return top N users by total words descending. Approach: HashMap<String, Integer> count words → MaxHeap (size N) or sort at end.

## Related Notes

- [[00. Overview]]
- [[Arrays]]
- [[Two Pointers and Sliding Window]]
- [[Time and Space Complexity]]