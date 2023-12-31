1. Goal of the 6.006:

    a. Solve computational problems

    b. Prove correctness 

    c. Argue efficiency

    d. Communication of these ideas

2. What is computation problem: 

        Is a set of inputs              Domain of outputs


                Problem: is a binary relation between inputs and outputs

3. Algorithm: 

        an algorithm map I -> O to one output    A problem input is sometimes called an --instance-- of the problem

        A (deterministic) algorithm is a procedure that maps inputs to single outputs.

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


5. --Model of computation--: A model of computation, in simple terms, is a conceptual framework or a set of rules and principles used to describe how a
        computer or a computational system works. It's like a --simplified representation-- that helps us understand and analyze how computations
        are performed, without getting into the nitty-gritty details of actual hardware or software

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


10. Difference between --Interfaces (API/ADT) vs Data structures--:

    - Interface says what you want to do (Specification - What data you want to store - What operations are supported)
    - Data structures says how to do it (Representation -       How to store it       - Algorithms to support operations)

11. --Sequence interfaces--:  

        a. --Static Sequence interface--: The number of items doesn't change.

            - items: x0, x1, ..., xn-1

            // operations
            
                - build(X): make new DS for items in X 
                - len(): return n 
                - iter_seq(): Output x0, x1, ..., xn-1 in sequence order. 
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

                 - items: x0, x1, ..., xn-1

            // operations
            
                - build(X): make new DS for items in X 
                - len(): return n 
                - iter_seq(): Output x0, x1, ..., xn-1 in sequence order. 
                - get_at(i): return xi (index i)
                - set_at(i,x): set xi to X
                - insert_at(i, x): make x new xi, shifting xi -> xi+1 -> xi+2 -> ... -> xn-1 -> xn 
                - delete_at(i): shift  xi <- xi+1 <- ... <- xn-2 <- xn-1
                - insert and delete at first or last
                - get_at and set_at at first or last 

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
                

                -- Dynamic Sequence Operations Comparison---:

                        Static Array                                     Linked list 

        insert and delete_at      Theta(n)                      insert_first and delete_first -> Theta(1)
                                                                get_at and set_at       -> Theta(n)

12. --Set interface--: 

        Set is just a container         

        -- operations: 

                --container--: 
                        a. build(A): given an iterable A, build sequence from items in A 
                        b. len(): return the number of stored items
                -- static--:
                        c. find(k): returned the stored item with key k 
                --dynamic--: 
                        d. insert(x): add x to set (replace item with key x.key if exists)
                        e. delete(k): remove and return the stored item with key k
                --Order--:
                        f. iter_ord(): return the store items one by one in key order 
                        g. find_min(): return the stored item with smallest key 
                        h. find_max(): return the stored item with largest key 
                        i. find_next(k): return the stored item with smallest key larger than k 
                        j. find_prev(k): return the stored item with largest key smaller than k
                
        Data structure                          Operations(Theta)
                                Container          Static          Dynamic                   Order 
                                build(A)           find(k)         insert(x)     find_min()           find_prev(k)
                                                                   delete(k)     find_max()           find_next(k)
        Unordered array             n               n                 n             n                      n

        Ordered array              nlogn            logn              n             1                      logn 

        direct access array         n                1                1             Unknown                 Unknown


13. --Sorting vocabulary--: 

        --Destructive--: Overwrite the input array 

        --In place--: Uses Theta(1) extra space         (This is common in solving problems)

14. --Sorting algorithms--: 

        a. --Permutation sort--: It is the Omega(n!*n) 

                def permutation_sort(A):
                        ''' Sort A '''
                        for B in permutations(A):
                                if is_sorted(B):
                                        return B 
        

                1. Enumerate perms -> Omega(n)
                2. Check if perm is sorted -> Theta(n)

                        for i in range(n-1):
                                return B[i] <= B[i+1]
                
        
        b. --Selection sort--: 

                Selection sort in nutshell: find the biggest element, swap it to the end. find the next big element swap it to the end minus 1. 

                [ATTENTION]
                It is possible to use two for loops for selection sort, but we are using --recursive-- to show efficiency and correctness.

                Steps for selection sort in a recursive way:
                        1. Find the biggest element in index <= i
                        2. Swap to the end of the list 
                        3. Sort 1, 2, ..., i - 1        (Repeat the two above steps)   
                        

                --Helper function for selection sort--

                def prefix_max(A, i):
                        '''Return the max element index in A[:i+1]'''
                        if i > 0: 
                                j = prefix_max(A, i - 1)
                                if A[i] < A[j]:
                                        return j 
                        return i
                
                - Proof of correctness by induction:

                        Base case -> is true 

                        inductive step: 

                        Assume our algorithm gives the prefix of max element in [:i]

                                We have two case for big element: 
                                        a. it is at element i 
                                        b. it has index < i

                - Checking the runtime:

                        (Runtime function) S(1) = Theta(1)
                                                .
                                                .
                                                .
                                           S(n) = S(n-1) + Theta(1)

                def selection_sort(A, i = None):
                        '''Sort A[:i+1]'''
                        if i is None: i = len(A) - 1
                        if i > 0: 
                                j = prefix_max(A, i)
                                A[i], A[j] = A[j], A[i]
                                selection_sort(A, i - 1)
        
        c. --Insertion sort--           (Find out about it)

        

        d. --Merge sort--: 

                def merge_sort(A, a = 0, b = None):
                        """Sort A[a:b]"""
                        if b is None: b = len(A)
                        if 1 < b - a: 
                        
15. For anyway to store items and for any algorithm we can't find an algorithm --faster than logn for find(k)--:

        --Comparison Model--: Comparison Model is a theoretical framework for analyzing and classifying algorithms based on the number of comparisons they
                make between elements in the input data.

                [ATTENTION]
                It's important to note that not all algorithms fit within the comparison model. There are non-comparison-based algorithms, 
                such as --counting sort-- and --radix sort-- for sorting, that do not rely solely on element comparisons and can achieve better 
                performance for specific types of data.

                You think of items as --black box--, we only can do comparison on the two items.

                        = , > , <       These are the operations
        
        [ATTENTION]
        All the sorting algorithms mentioned above are --Comparison Sorting Algorithm--.

        We can view and model algorithms in Comparison model as a --Decision Tree--.

        --Decision Trees--: 
                Decision trees are often used to model the behavior of comparison-based algorithms. 
                --Each node in the tree represents a comparison between two elements--,
                 and --the branches represent the possible outcomes of the comparison--.
                Analyzing the height of the decision tree provides insights into the lower bounds for the problem.
        

                [The height of Decision tree is logn in comparison model]

                To get time complexity lower than logn we can use --direct access array--
                in the direct access, we store item with key k at k index.

                [ATTENTION]
                Direct access arrays cause space limitation             u (maximum key) <= w (word size)

                



