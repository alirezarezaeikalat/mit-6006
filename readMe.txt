1. Goal of the 6.006:

    a. Solve computational problems

    b. Prove correctness 

    c. Argue efficiency

    d. Communication of these ideas

2. What is computation problem: 

        Is a set of inputs              Domain of outputs


                Problem: is a binary relation between inputs and outputs

3. Algorithm: 

        an algorithm map I -> O to one output

4. Birthday problem: 

        a. maintain a record of birthday 
        b. Interview students in some orders (It means check if the same record exists, return it or otherwise add the record)
        c. if the students finished without the match, return no match.


        -- proof of correctness: 


            Predicate: If k students contains match the algorithm returns a match before Interviewing the k+1 student.

            Base case: P(0) done

            Assume IH is true for k

            P(k + 1): We have two case: 

                case 1: if the k has a match, already returned by induction

                case 2: if the new student match the algorithm will returns it, otherwise it returns no match.
        
        -- Proof of efficiency: 


5. Model of computation: What computers can do in a fixed amount of time

        Word-RAM model: randomly access memory in constant time

            --Word--: how big a chunk a cpu can brings on and operate on.
        

            CPU operations in constant time: 
                
                a. Integer arithmetics
                b. Logical operations 
                c. Bitwise operations
                d. Read and write in memory

////////////////////////////// Asymptotic notation /////////////////

1 < logn < sqrt(n) < n < nlogn < n^2 < n^3 < ... < 2^n < 3^n < ...

6. Asymptotic notation is used to show --class of function-- in simple form.

7. Big O notation is one form (upper bound):

        --Definition--: The function f(n) = O(g(n)) if and only if exits constants c and n0 

                f(n) <= c*g(n)      for every n >= n0
        
        --Example: f(n) = 2n + 3        => f(n) = O(n)      [ATTENTION] f(n) = O(n) is also true

                2n + 3 <= 10 * n       for every n >= 1
                   |       |   |                      |
                  f(n)     c  g(n)                    n0
        
        --ATTENTION: 
                a. for the above example, all functions after n (like nlogn, n^2, ...) become upper bound 
                b. all functions before n (like sqrt(n), logn, and 1) become lower bound 
                c. and n become average bound 

8. Big omega notation (lower bound)

        --Definition--: The function f(n) = Omega(g(n)) if and only if exists constants c and n0 
                    
                f(n) >= c * g(n)    for every n >= n0 
        
        --Example: f(n) = 2n + 3                    => f(n) = Omega(n)      [ATTENTION]  f(n) = Omega(logn) is also true

                2n + 3 >= 1 * n    for every n >= 1             
                   |      |   |                   |
                   f(n)   c   g(n)                n0
9. Big Theta notation (Average bound)
        
        --Definition--: The function f(n) = Theta(g(n)) if and only if exists constants c1, c2 and n0

                such that       c1 * g(n) <= f(n) <= c2 * g(n)      for every n >= n0
        
          --Example: f(n) = 2n + 3                    => f(n) = Theta(n)     

              1 * n <= 2n + 3 <= 5 * n    for every n >= 1             
              |           |      |   |                   |
              c0          f(n)   c1  g(n)                n0

////////////////////////////// Data structures and Dynamic programming /////////////////

10. Difference between Interfaces (API/ADT) vs Data structures:

    - Interface says what you want to do (Specification - What data you want to store - What operations are supported)
    - Data structures says how to do it (Representation -       How to store it       - Algorithms to support operations)

11. Sequence interfaces:  

        a. --Static Sequence interface--: The number of items doesn't change.

            - items: x0, x1, ..., xn

            // operations
            
                - build(X): make new DS for items in X 
                - len(): return n 
                - iter_seq(): Output x0, x1, ..., xn in sequence order. 
                - get_at(i): return xi (index i)
                - set_at(i,x): set xi to X

            1. --Static array-- (Solution to above interface): (This relates to our model of computation --Word RAM--: memory = array of w-bits words)
                --Array--: --Consecutive-- chunk of memory.

                It is consecutive => array[i] = memory[address(array) + i] => access array at --constant time Theta(1)--

                    - Theta(1): for get_at, set_at, and len
                    - Theta(n): for build, and iter_seq
                
                --How to build an array at first place--: 
                    --Memory allocation model--: 
                        allocate array of size of n in Theta(n)

                [ATTENTION]
                To assume array access takes constant time we need W (word size) >= logn   (search for the reason in ChatGPT)

        b. Dynamic sequence interface: The numbers of items change.

                 - items: x0, x1, ..., xn

            // operations
            
                - build(X): make new DS for items in X 
                - len(): return n 
                - iter_seq(): Output x0, x1, ..., xn in sequence order. 
                - get_at(i): return xi (index i)
                - set_at(i,x): set xi to X
                - insert_at(i, x): make x new xi, shifting xi -> xi+1 -> xi+2 -> ... -> xn-1 -> xn 
                - delete_at(i): shift  xi <- xi+1 <- ... <- xn-2 <- xn-1
                - insert ant delete at first or last

            1. --Linked list-- (Solution to above interface):

                    each node has : {
                        item;
                        next;
                    }

                    We have --head--, --len--, We can add --tail-- (We call this --data structure augmentation-- (add more info about DS))
            
            2. --Dynamic arrays-- (What python called list, inside python interpreter):

                - Relax constraint size(array) = n
                - How much make the array bigger? make it bigger --roughly-- from algorithm perspective, --roughly-- means throw constant on it.
                - Enforce size = theta(n)   & >= n
                - Maintain A[i] = xi            (There are some blanks at the end)
                - insert_at_last -> A[len] = x, len +=1


                        A (pointer)
                        len 
                        size (Actual size of array)
                
                - If len = size we don't have space: 

                        [ATTENTION]
                        With static arrays we make our array bigger --every time-- we want to add an item. We dynamic arrays we do make the array bigger 
                                --sometimes--.
                        
                        allocate new array of 2 * size

                        - n insert_at_last from empty array:

                                resize at n = 1, 2, 4, 16, ...

                                => resize cost is linear each time = Theta(1 + 2 + 4 + 8 + 16) = Theta(Sigma_i=0_i=logn(2^i)) = Theta(2^logn - 1)

                                == Theta(n)     Linear time for --all of the operations--, It kind of --like constant each--         
                
                -- Amortization--:      Operation takes T(n) amortized time if any k operations takes <= k.T(n) time

                        So in the above example T(1)    (Constant amortized)

                                amortized is a particular kind of averaging, averaging --over the sequence of operations--

