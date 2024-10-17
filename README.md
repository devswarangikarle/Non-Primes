# Non-Primes

You are given an array A of size N. Find a pair of indices (i,j) such that:

1 ≤ i, j ≤ N, i ≠ j;
Ai + Aj is not prime;
In case multiple (i, j) pairs are valid, you can print any one. If it is impossible to find such (i, j), output −1 instead.
Note: The given number x (x ≥ 2) is said to be prime if it has exactly 2 positive divisors - 1, and x itself.
Input
The first line of input will contain a single integer T, denoting the number of test cases.
Each test case consists of multiple lines of input.
- The first line contains N - the size of the array A.
- The second line contains N space-separated integers, A1, A2, ... AN, denoting the elements of an array.

Constraints
1 ≤ T ≤ 2.104
2 ≤ N ≤ 2.105
1 ≤ Ai ≤ 100
Note: The sum of N over all test cases does not exceed 2x105.
Output
For each test case, output:
* Two space-separated integers i and j, denoting a valid pair (i, j).
* -1 if there exist no valid pair (i, j).
Note: In case multiple (i, j) pairs are valid, you can print any one.

import sys
input = sys.stdin.read

def sieve_of_eratosthenes(limit):

    is_prime = [True] * (limit + 1)
    is_prime[0] = is_prime[1] = False
    for num in range(2, int(limit**0.5) + 1):
        if is_prime[num]:
            for multiple in range(num * num, limit + 1, num):
                is_prime[multiple] = False
    return is_prime

def find_non_prime_pair(A, N, is_prime):

    freq = [0] * 101  
    for num in A:
        freq[num] += 1

    for i in range(1, 101):
        if freq[i] > 0:
            for j in range(i + 1, 101):
                if freq[j] > 0 and not is_prime[i + j]:
                    first_i = A.index(i) + 1
                    first_j = A.index(j) + 1
                    return (first_i, first_j)
    
    for i in range(1, 101):
        if freq[i] > 1 and not is_prime[2 * i]:
            first_i = A.index(i) + 1
            second_i = A.index(i, first_i) + 1
            return (first_i, second_i)
    
    return -1

def main():
    is_prime = sieve_of_eratosthenes(200)
    
    data = input().splitlines()
    idx = 0
    T = int(data[idx])
    idx += 1
    result = []
    
    for _ in range(T):
        N = int(data[idx])
        idx += 1
        A = list(map(int, data[idx].split()))
        idx += 1
        
        pair = find_non_prime_pair(A, N, is_prime)
        if pair == -1:
            result.append("-1")
        else:
            result.append(f"{pair[0]} {pair[1]}")
    
    sys.stdout.write("\n".join(result) + "\n")

if __name__ == "__main__":
    main()
