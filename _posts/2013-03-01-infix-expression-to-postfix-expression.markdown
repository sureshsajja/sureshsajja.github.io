---
author: sureshsajja
comments: true
date: 2013-03-01 21:32:29+00:00
layout: post
link: http://coderevisited.com/infix-expression-to-postfix-expression/
slug: infix-expression-to-postfix-expression
title: Infix expression to Postfix expression
wordpress_id: 424
categories:
- Java
---

This post shows sample implementations for
1. Converting an infix expression to postfix expression (Reverse polish notation)
2. Evaluating postfix expression



### Infix to PostFix notation


To convert from infix to postfix notation, I have implemented Dijkstra's shunting-yard algorithm. For detailed algorithm, please take a look at [wiki page](http://en.wikipedia.org/wiki/Shunting-yard_algorithm).



### Evaluating postfix expression


This program evaluates postfix expresions using a stack.

The following is sample implementation in java
 

    
    package stack;
    
    import java.util.HashMap;
    import java.util.Iterator;
    import java.util.Map;
    import java.util.StringTokenizer;
    
    import queue.LinkedListQueue;
    import queue.Queue;
    
    /**
     * This program implements Dijkstra's shunting-yard algorithm to convert an
     * infix expression into a postfix expression. This program supports operator
     * precedence, including both left and right associative operators.
     * 
     * http://en.wikipedia.org/wiki/Shunting-yard_algorithm
     * 
     * @author sureshsajja
     * 
     */
    public class InFixToPostFixExpression {
    
    	private static Map<String, Operators> operatorsMap = new HashMap<>();
    	private static final int LEFT_ASSOCIATIVITY = 0;
    
    	static {
    		operatorsMap.put("+", Operators.ADD);
    		operatorsMap.put("-", Operators.SUBTRACT);
    		operatorsMap.put("*", Operators.MULTIPLY);
    		operatorsMap.put("/", Operators.DIVIDE);
    	}
    
    	private enum Operators {
    		ADD {
    			@Override
    			public int getAssociativity() {
    				return LEFT_ASSOCIATIVITY;
    			}
    
    			@Override
    			public int getPrecedence() {
    				return 2;
    			}
    
    			@Override
    			public int evaluate(int d1, int d2) {
    				return d1 + d2;
    			}
    		},
    		SUBTRACT {
    			@Override
    			public int getAssociativity() {
    				return LEFT_ASSOCIATIVITY;
    			}
    
    			@Override
    			public int getPrecedence() {
    				return 2;
    			}
    
    			@Override
    			public int evaluate(int d1, int d2) {
    				return d1 - d2;
    			}
    		},
    		MULTIPLY {
    			@Override
    			public int getAssociativity() {
    				return LEFT_ASSOCIATIVITY;
    			}
    
    			@Override
    			public int getPrecedence() {
    				return 3;
    			}
    
    			@Override
    			public int evaluate(int d1, int d2) {
    				return d1 * d2;
    			}
    		},
    		DIVIDE
    
    		{
    			@Override
    			public int getAssociativity() {
    				return LEFT_ASSOCIATIVITY;
    			}
    
    			@Override
    			public int getPrecedence() {
    				return 3;
    			}
    
    			@Override
    			public int evaluate(int d1, int d2) {
    				return d1 / d2;
    			}
    		};
    		public abstract int getAssociativity();
    
    		public abstract int getPrecedence();
    
    		public abstract int evaluate(int d1, int d2);
    	}
    
    	public static String inFixToPostFix(String s) {
    
    		Queue<String> output = new LinkedListQueue<>();
    		Stack<String> stack = new LinkedListStack<>();
    
    		StringTokenizer st = new StringTokenizer(s, " ");
    		while (st.hasMoreTokens()) {
    			String token = st.nextToken();
    			if (isNumber(token)) {
    				output.enqueue(token);
    			} else if (isOperator(token)) {
    				while (!stack.isEmpty()) {
    					String peek = stack.peek();
    					if (isTopwithHigerPrecedence(peek, token)) {
    						output.enqueue(stack.pop());
    					} else
    						break;
    				}
    				stack.push(token);
    			} else if (isLeftParantheses(token)) {
    				stack.push(token);
    			} else if (isRightParantheses(token)) {
    				boolean isLeftFound = false;
    				while (!stack.isEmpty()) {
    					String top = stack.pop();
    					if (!isLeftParantheses(top)) {
    						output.enqueue(top);
    					} else {
    						isLeftFound = true;
    						break;
    					}
    				}
    				if (!isLeftFound)
    					throw new RuntimeException("Parantheses are not balanced");
    
    			}
    
    		}
    
    		while (!stack.isEmpty()) {
    			String top = stack.pop();
    			if (isLeftParantheses(top) || isRightParantheses(top)) {
    				throw new RuntimeException("Parantheses are not balanced");
    			}
    			output.enqueue(top);
    		}
    
    		Iterator<String> itr = output.iterator();
    
    		StringBuilder sb = new StringBuilder();
    		while (itr.hasNext()) {
    			sb.append(itr.next());
    			sb.append(" ");
    		}
    
    		return sb.toString();
    	}
    
    	public static int postFixEvaluation(String expr) {
    
    		Stack<Integer> stack = new LinkedListStack<>();
    		StringTokenizer st = new StringTokenizer(expr, " ");
    		while (st.hasMoreTokens()) {
    			String token = st.nextToken();
    			if (isNumber(token)) {
    				stack.push(Integer.valueOf(token));
    			} else if (isOperator(token)) {
    				int op2 = stack.pop();
    				int op1 = stack.pop();
    				int result = operatorsMap.get(token).evaluate(op1, op2);
    				stack.push(result);
    			}
    		}
    
    		return stack.pop();
    
    	}
    
    	private static boolean isTopwithHigerPrecedence(String peek, String token) {
    		if (!isOperator(peek))
    			return false;
    
    		Operators op1 = operatorsMap.get(peek);
    		Operators op2 = operatorsMap.get(token);
    
    		if (op2.getAssociativity() == LEFT_ASSOCIATIVITY
    				&& op1.getPrecedence() >= op2.getPrecedence())
    			return true;
    		else if (op1.getPrecedence() >= op2.getPrecedence())
    			return true;
    		else
    			return false;
    
    	}
    
    	private static boolean isRightParantheses(String token) {
    		return token.equals(")");
    	}
    
    	private static boolean isLeftParantheses(String token) {
    		return token.equals("(");
    	}
    
    	private static boolean isOperator(String token) {
    		if (operatorsMap.containsKey(token))
    			return true;
    		return false;
    	}
    
    	private static boolean isNumber(String token) {
    		return token.matches("\\d+");
    	}
    
    	public static void main(String[] args) {
    		String s = "3 + 5 * 6 - 7 * ( 8 + 5 )";
    		System.out.println(s+ " InFix To PostFix " + inFixToPostFix(s));
    		System.out.println(s+ " PostFix evalution " + postFixEvaluation(inFixToPostFix(s)));
    
    	}
    
    }
    



For other libraries used in the above program, please refer to GitHub repo [here](https://github.com/sureshsajja)
