# Trie

![](https://github.com/shamy1st/data-structure/blob/main/images/trie.png)
![](https://github.com/shamy1st/data-structure/blob/main/images/trie-complexity.png)

--                 | search              | add                 | remove
-------------------|---------------------|---------------------|-----------------------
Trie               | O(L)                | O(L)                | O(L)

* **Traversals**
  * **Pre-Order** print word in forward order.
  * **Post-Order** print word in reverse order.

        public class Main {
            public static void main(String[] args) {
                Trie trie = new Trie();
                trie.insert("car");
                trie.insert("card");
                trie.insert("care");
                trie.insert("careful");
                trie.insert("egg");
                System.out.println(trie.findWords(null));
            }
        }

        public class Trie {
            private class Node {
                private char value;
                private HashMap<Character, Node> children;
                private boolean endOfWord;

                public Node(char value) {
                    this.value = value;
                    children = new HashMap<>();
                }

                public boolean hasChild(char ch) {
                    return children.containsKey(ch);
                }

                public void addChild(char ch) {
                    children.put(ch, new Node(ch));
                }

                public Node getChild(char ch) {
                    return children.get(ch);
                }

                public Node[] getChildren() {
                    return children.values().toArray(new Node[0]);
                }

                public boolean hasChildren() {
                    return !children.isEmpty();
                }

                public void setEndOfWord(boolean endOfWord) {
                    this.endOfWord = endOfWord;
                }

                public boolean isEndOfWord() {
                    return endOfWord;
                }

                @Override
                public String toString() {
                    return "Node{" + value + "}";
                }

                public void removeChild(char ch) {
                    children.remove(ch);
                }
            }

            private Node root;

            public Trie() {
                root = new Node('\0');
            }

            public void insert(String word) {
                Node current = root;
                for(char ch : word.toCharArray()) {
                    if(!current.hasChild(ch))
                        current.addChild(ch);
                    current = current.getChild(ch);
                }
                current.setEndOfWord(true);
            }

            public boolean contains(String word) {
                if(word == null)
                    return false;

                Node current = root;
                for(char ch : word.toCharArray()) {
                    if(!current.hasChild(ch))
                        return false;
                    current = current.getChild(ch);
                }

                return current.isEndOfWord();
            }

            public void traverse() {
                traversePreOrder(root);
            }

            private void traversePreOrder(Node node) {
                for(Node child : node.getChildren()) {
                    System.out.println(child.value);
                    traversePreOrder(child);
                }
            }

            private void traversePostOrder(Node node) {
                for(Node child : node.getChildren()) {
                    traversePostOrder(child);
                    System.out.println(child.value);
                }
            }

            public void remove(String word) {
                if(word == null)
                    return;

                remove(root, word, 0);
            }

            private void remove(Node node, String word, int index) {
                if(index == word.length()) {
                    node.setEndOfWord(false);
                    return;
                }

                Node child = node.getChild(word.charAt(index));
                if(child == null)
                    return;

                remove(child, word, index+1);
                if(!child.hasChildren() && !child.isEndOfWord())
                    node.removeChild(child.value);
            }

            public List<String> findWords(String prefix) {
                if(prefix == null)
                    return null;

                Node lastNode = findLastNodeOf(prefix);
                List<String> list = new ArrayList<>();
                findWords(lastNode, prefix, list);
                return list;
            }

            private void findWords(Node node, String prefix, List<String> list) {
                if(node == null)
                    return;

                if(node.isEndOfWord())
                    list.add(prefix);

                for(Node child : node.getChildren()) {
                    findWords(child, prefix + child.value, list);
                }
            }

            private Node findLastNodeOf(String prefix) {
                Node current = root;
                for(char ch : prefix.toCharArray()) {
                    Node child = current.getChild(ch);
                    if(child == null)
                        return null;
                    current = child;
                }
                return current;
            }
        }
