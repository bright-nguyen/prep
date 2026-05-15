# Reorder Data in Log Files

## Description:
Given an array of logs, where each log is a space-delimited string with an identifier as the first word. The logs are categorized into:
- Letter-logs: containing words with lowercase letters after the identifier
- Digit-logs: containing only digits after the identifier

Reorder these logs so that:
- All letter-logs come before digit-logs
- Letter-logs are sorted lexicographically by their content, and ties are broken by their identifiers
- Digit-logs maintain their relative ordering

Example 1:
```
Input: 
logs = ["dig1 8 1 5 1", "let1 art can", "dig2 3 6", "let2 own kit dig", "let3 art zero"]
Output: 
["let1 art can","let3 art zero","let2 own kit dig","dig1 8 1 5 1","dig2 3 6"]
```
Example 2:
```
Input: 
logs = ["a1 9 2 3 1","g1 act car","zo4 4 7","ab1 off key dog","a8 act zoo"]
Output: 
["g1 act car","a8 act zoo","ab1 off key dog","a1 9 2 3 1","zo4 4 7"]
```
Constraints:
- `1 <= logs.length <= 100`
- `3 <= logs[i].length <= 100`
- All identifiers are unique
- Each word after identifier is separated by a space

## Solution:
### Overview
This problem tests the ability to handle custom sorting logic with multiple criteria and string manipulation. The key is to create a sorting key function that properly handles both letter and digit logs while maintaining the required ordering rules.

#### Time Complexity
O(m * n * log n) where n is the number of logs and m is the maximum length of a log. This comes from the sorting operation and string comparisons.

#### Space Complexity
O(m * n) where n is the number of logs and m is the maximum length of a log, required for storing the sorted array and temporary string splits.


```javascript
const solution = (logs) => {
    const letterLogs = [];
    const digitLogs = [];

    for (const log of logs) {
        const spaceIdx = log.indexOf(' ');
        const rest = log.substring(spaceIdx + 1);
        if (rest[0] >= '0' && rest[0] <= '9') {
            digitLogs.push(log);
        } else {
            letterLogs.push(log);
        }
    }

    // Sort letter-logs by content, then by identifier
    letterLogs.sort((a, b) => {
        const spaceA = a.indexOf(' ');
        const idA = a.substring(0, spaceA);
        const restA = a.substring(spaceA + 1);

        const spaceB = b.indexOf(' ');
        const idB = b.substring(0, spaceB);
        const restB = b.substring(spaceB + 1);

        if (restA < restB) return -1;
        if (restA > restB) return 1;
        if (idA < idB) return -1;
        if (idA > idB) return 1;
        return 0;
    });

    return letterLogs.concat(digitLogs);
}
```

Note: Uses `indexOf(' ')` and `substring()` instead of `split()` to avoid creating arrays on every comparison.
