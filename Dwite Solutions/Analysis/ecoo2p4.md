The ```"MINE - YOU LOSE"``` case is obviously very easy. So is the ```"NO MINE - ... SURROUNDING IT"``` case. The only nontrivial case is the one in 

which a large number of squares are revealed because the clicked square and all its neighbours are unmined. This is best tackled using a

flood fill; once we reach a square that has at least one mine surrounding it, we stop exploring from that square. Don't forget to restore 

the grid back to its original configuration after each test case.
