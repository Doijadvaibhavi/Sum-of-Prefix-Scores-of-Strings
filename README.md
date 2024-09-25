# Sum-of-Prefix-Scores-of-Strings

You are given an array words of size n consisting of non-empty strings.

We define the score of a string term as the number of strings words[i] such that term is a prefix of words[i].

For example, if words = ["a", "ab", "abc", "cab"], then the score of "ab" is 2, since "ab" is a prefix of both "ab" and "abc".
Return an array answer of size n where answer[i] is the sum of scores of every non-empty prefix of words[i].

Note that a string is considered as a prefix of itself.

 

Example 1:

Input: words = ["abc","ab","bc","b"]
Output: [5,4,3,2]
Explanation: The answer for each string is the following:
- "abc" has 3 prefixes: "a", "ab", and "abc".
- There are 2 strings with the prefix "a", 2 strings with the prefix "ab", and 1 string with the prefix "abc".
The total is answer[0] = 2 + 2 + 1 = 5.
- "ab" has 2 prefixes: "a" and "ab".
- There are 2 strings with the prefix "a", and 2 strings with the prefix "ab".
The total is answer[1] = 2 + 2 = 4.
- "bc" has 2 prefixes: "b" and "bc".
- There are 2 strings with the prefix "b", and 1 string with the prefix "bc".
The total is answer[2] = 2 + 1 = 3.
- "b" has 1 prefix: "b".
- There are 2 strings with the prefix "b".
The total is answer[3] = 2.
Example 2:

Input: words = ["abcd"]
Output: [4]
Explanation:
"abcd" has 4 prefixes: "a", "ab", "abc", and "abcd".
Each prefix has a score of one, so the total is answer[0] = 1 + 1 + 1 + 1 = 4.
 

Constraints:

1 <= words.length <= 1000

1 <= words[i].length <= 1000

words[i] consists of lowercase English letters.

# SOLUTION 

Intuition
The problem can be effectively solved using a Trie (prefix tree) data structure. The Trie structure allows us to efficiently store and traverse the prefixes of words. Each node in the Trie keeps track of how many times a particular prefix has appeared. This enables us to calculate the prefix scores for each word by summing the count of nodes that match the word's prefix.

The main idea is to:

Insert each word into the Trie, where each node tracks how many times a prefix has been encountered.
Query the prefix score of each word by summing the prefix counts while traversing the Trie.
Approach
Trie Construction:

Each node of the Trie contains a count (to store the number of words that pass through this node) and an array to hold child nodes corresponding to each character (a-z).
When inserting a word, for each character in the word:
If the node for the character doesn’t exist, create it.
Increment the count for the node to indicate that a word has passed through this node.
Prefix Score Calculation:

To calculate the prefix score for a word, traverse the Trie for each character in the word.
At each step, add the count of the node to the result, which will give the total number of words that share the prefix.
Efficiency:

Insertion and prefix calculation are both O(L) operations, where L is the length of the word. This ensures efficient processing even for larger input sizes.
Steps:

Insert all words into the Trie:
For each word in the input list, add it to the Trie, updating the count of each node.
Calculate the prefix scores:
For each word, traverse the Trie and sum the counts of the nodes along the way to calculate the total prefix score for that word.

Complexity

Time complexity:O(N*W)

Space complexity:O(N*W)

# JAVA CODE 

class Node {

    int count = 0;
    
    Node[] list = new Node[26];

    public boolean containKey(char ch) {
    
        return list[ch - 'a'] != null;
    }

    public Node get(char ch) {
    
        return list[ch - 'a'];
    }

    public void put(char ch, Node new_node) {
    
        list[ch - 'a'] = new_node;
    }

    public void inc(char ch) {
    
        list[ch - 'a'].count++;
    }

    public int retCount(char ch) {
    
        return list[ch - 'a'].count;
    }
}

class Solution {

    private Node root;

    public Solution() {
    
        root = new Node();
    }

    public void insert(String word) {
    
        Node node = root;
        
        for (char ch : word.toCharArray()) {
        
            if (!node.containKey(ch)) {
            
                node.put(ch, new Node());
            }
            
            node.inc(ch);
            
            node = node.get(ch);
        }
    }

    public int search(String word) {
    
        Node node = root;
        
        int preCount = 0;
        
        for (char ch : word.toCharArray()) {
        
            preCount += node.retCount(ch);
            
            node = node.get(ch);
        }
        
        return preCount;
    }

    public int[] sumPrefixScores(String[] words) {
    
        // This problem can be solved using the trie data structure
        
        for (String word : words) {
        
            insert(word);
        }
        
        int n = words.length;
        
        int[] res = new int[n];
        
        for (int i = 0; i < n; i++) {
        
            res[i] = search(words[i]);
        }
        
        return res;
    }
}
