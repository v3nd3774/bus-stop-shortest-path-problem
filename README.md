# bus-stop-shortest-path-problem
repo to store code for use on codesignal freestyle with clojure solution to bus stop shortest path problem

Original console content
```clojure
; Input:
; positive integers
; first bus keeps going 1, 2, 7 forever same with bus two (second list) but with it's respective stops
; routes = [[1, 2, 7], [3, 6, 7]]  // list of lists
; source = 1
; destination = 7

; output:
; Minimum number of hops the user has to make to go from source to destination

; they are on the same bus
; Output#1:
; 0

; Input#2
; routes = [[1, 2, 7], [3, 6, 7]]  // list of lists
; source = 1
; destination = 6

; Output#2:
; 1

; Input#3
; routes = [[1, 2, 7], [3, 6, 7], , [10, 6, 8], [11]]  // list of lists
; source = 1
; destination = 8

; Output#3:
; 2

; Input#4
; routes = [[1, 2, 7], [3, 6, 7], , [10, 6, 8], [11]]  // list of lists
; source = 1
; destination = 11

; Output#4: -1
; invalid
(require '[clojure.set])
(def input-one [[1 2 7] [3 6 7]])
(def debug? true)
(defn get-pairs [bus]
    (let [first-half (loop [[lhead & ltail] bus
                            acc-outer #{}]
                            (if (nil? lhead)
                                (let []
                                (if debug?
                                    (println (str "half of the edges on this bus = " acc-outer)))
                                    acc-outer)
                                (recur ltail (clojure.set/union acc-outer (loop [[rhead & rtail] (cons lhead ltail)
                                                                            acc-inner #{}]   
                                                                        (if debug?
                                                                            (let []
                                                                            (println (str "left list = " lhead " :: " ltail))
                                                                            (println (str "acc-outer: " acc-outer))
                                                                            (println (str "input bus: " bus))
                                                                            (println (str "right list = " rhead " :: " rtail))
                                                                            (println (str "acc-inner: " acc-inner))))
                                                                        (if (nil? rhead)
                                                                            acc-inner
                                                                            (recur rtail (clojure.set/union #{[lhead rhead]} acc-inner))))))))
          remaining-edges (loop [[[src dest] & tail] first-half
                                 acc #{}]
                                (if debug?
                                    (let []
                                    (println (str "src, dest = " src " --> " dest))
                                    (println (str "acc = " acc))))
                                (if (nil? src)
                                    acc
                                    (if (= src dest)
                                        (recur tail acc)
                                        (recur tail (clojure.set/union acc #{[dest src]})))))
           all-edges-both-ways (clojure.set/union first-half remaining-edges)]
            (if debug?
                (println (str "all the edges on this bus = " all-edges-both-ways)))
          all-edges-both-ways))
                    
(get-pairs (first input-one))
(defn solution [input-list]
    (let [V (set (flatten input-list))
          E (reduce (fn [acc bus]
                        (clojure.set/union acc (get-pairs bus)))
                    #{}
                    input-list)]
            (if debug?
                (let []
                (println (str "input list: " input-list))
                (println (str "Stops: " V))
                (println (str "Edges: " E))))))
    
(solution input-one)
```
