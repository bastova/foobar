# foobar

// Undercover_underground in Java (find number of labeled, simple, connected graphs with n nodes and k edges

package com.google.challenges; 

import java.util.HashMap;
import java.util.ArrayList;
import java.math.BigInteger;

public class Answer {   
    public static String answer(int N, int K) { 

        Answer answer = new Answer();
        return answer.getAnswer(N,K);

    } 
    
    private HashMap<ArrayList<Long>, BigInteger> bCache;
    private HashMap<ArrayList<Long>, BigInteger> gCache;
    
    public String getAnswer(int N, int K) {
        bCache = new HashMap<>();
        gCache = new HashMap<>();
        
        return calculateGraphs(N,K).toString();
    }
    
    private BigInteger calculateGraphs(int N, int K) {
        
        ArrayList<Long> pair = new ArrayList<>();
        pair.add((long)N); pair.add((long)K);
        
        if (gCache.containsKey(pair)) return gCache.get(pair);
        
        long total = (long) (N * ( N - 1) / 2);
        BigInteger coef = BigInteger.valueOf(0);
        
        if (K == total) coef = BigInteger.valueOf(1);
        else if (K == N - 1) {
        	coef = (N - 2 == -1) ? BigInteger.valueOf(1) : BigInteger.valueOf(N).pow(N-2);
        } else {
        	
	        coef = choose( total,(long)K);
	        
	        if (K < total - N + 2) {
	        	
	            for (int i = 1; i < N; i++) {
	                BigInteger b = choose(N - 1, i - 1);
	                int limit = (i * ( i - 1) >> 1 < K) ? i * ( i - 1) >> 1 : K;
	                for (int j = i - 1; j < limit + 1; j++) {
	                    BigInteger g = choose((N - i) * (N - i - 1) / 2, K - j);
	                    BigInteger c = calculateGraphs(i,j);
	                    BigInteger p = b.multiply(g).multiply(c);
	                    coef = coef.subtract(p);
	                }
	            }
	        }
        	
        }
        
        gCache.put(pair,coef);
        return coef;
    }
    
    public BigInteger choose(long total, long choose){
        ArrayList<Long> pair = new ArrayList<>();
        pair.add(total); pair.add(choose);
        if (bCache.containsKey(pair)) 
            return bCache.get(pair);
        if(total < choose) {
            bCache.put(pair,BigInteger.valueOf(0));
            return BigInteger.valueOf(0);
        }
        if(choose == 0 || choose == total) {
            bCache.put(pair,BigInteger.valueOf(1));
            return BigInteger.valueOf(1);
        }
        if(choose == 1 || choose == total - 1) {
            bCache.put(pair,BigInteger.valueOf(total));
            return BigInteger.valueOf(total);
        }
        bCache.put(pair,choose(total-1,choose-1).add(choose(total-1,choose)));
        return bCache.get(pair);
    }
    
}
