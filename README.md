# number-of-atoms
class Solution {
    public String countOfAtoms(String formula) {
        Deque<Map<String, Integer>> stack = new LinkedList<>();
        stack.push(new TreeMap<>());
        
        int i = 0, n = formula.length();
        while (i < n) {
            char ch = formula.charAt(i);
            
            if (ch == '(') {
                stack.push(new TreeMap<>());
                i++;
            } else if (ch == ')') {
                Map<String, Integer> top = stack.pop();
                int iStart = ++i, multiplicity = 1;
                while (i < n && Character.isDigit(formula.charAt(i))) i++;
                if (i > iStart) multiplicity = Integer.parseInt(formula.substring(iStart, i));
                
                for (String name : top.keySet()) {
                    int v = top.get(name);
                    stack.peek().put(name, stack.peek().getOrDefault(name, 0) + v * multiplicity);
                }
            } else {
                int iStart = i++;
                while (i < n && Character.isLowerCase(formula.charAt(i))) i++;
                String name = formula.substring(iStart, i);
                iStart = i;
                while (i < n && Character.isDigit(formula.charAt(i))) i++;
                int multiplicity = i > iStart ? Integer.parseInt(formula.substring(iStart, i)) : 1;
                stack.peek().put(name, stack.peek().getOrDefault(name, 0) + multiplicity);
            }
        }
        
        StringBuilder sb = new StringBuilder();
        for (String name : stack.peek().keySet()) {
            sb.append(name);
            int count = stack.peek().get(name);
            if (count > 1) sb.append(count);
        }
        return sb.toString();
    }
}
