# 1. Intelligent agents, environment, and structure of agents:

### Define an agent and a rational agent.

- Agent je hocico (entita), co dokaze vnimat prostredie cez senzory a nasledne kona cez actuaktory.

- Racionalny agent je agent, ktory si vybera akciu, ktora maximalizuje jeho ocakavanu mieru uspesnosti (utility). $action = argmax_a E(utility(p,a)))$, kde $p$ je percept (nieco, co je vnimane), $a$ su mozne akcie, $E$ ocakavany uzitok, utility je uzitkova funkcia

### Explain properties of environment (partial/full observability, deterministic/stochastic, episodic/sequential, static/dynamic,discrete/continuous, single/multi-agent).

- Observability (pozorovatelnost):
  - full (uplne): agent ma pristup ku uplnemu stavu v prostredi (napr. sach)
  - partial (ciastocne): agent ma pristup len k casti casti prostredia  (napr. robot s lokalnymi senzormi)
 
- Deterministicke prostredie:
  - nasledujuci stav je jednoznacne urceny podla vykonanej akcie v aktualnom stave

- Stochasticke prostredie:
  - nasledujuci stav nie je jednoznacne urceny podla vykonanej akcie v aktualnom stave

- Episodicke prostredie
  - kazdy krok nezavisi na predchadzajucom (napr. klasifikacia obrazkov)

- Sekvencne prostredie:
  - rozhodnodnutie ovplyvnuje buduce percepty/odmeny (napr. hranie hier)

- Staticke prostredie:
  - svet sa nemeni medzi agentovymi akciami

- Dynamicke prostredie:
  - prostredie sa moze menit samo od seba

- Diskretne prostredie:
  - konecny alebo spocetny pocet stavovy priestor

- Spojite (kontinualne) prostredie:
  - nekonecne vela stavov/akcii

- Single-agent prostredie:
  - jeden agent ovplyvnuje prostredie

- Multi-agent prostredie:
  - viac autonomnych agentov, kooperacia/konkurencia (napr. futbal)

### Explain and compare reflex and goal-based agent architectures and possible representations of states (atomic, factored, structured)

- Reflex agent:
  - vyberie akciu podla aktualneho stavu sveta podla pozorovani. Vpodstate if-then agent, pozrie sa na aktualny svet (prostredie), pozrie sa aku akciu ma vykonat a vykona ju

- Goal-based agent:
  - agent moze byt viac flexibilny ak predpokladaju, ze maju nejaky ciel (vhodne stavy) okrem aktualneho stavu.

  - berie do uvahy buduce stavy, pozrie sa na aktualny svet, pozrie sa ako by sa svet evolvoval, ak by vykonal akciu $A$, a porovna s jeho golom, ci by mu akcia $A$ pomohla.
  
  - Pouzivane napr. v planovani a hladani.

- Problem solving agent je typ goal-based agenta, ktory pouziva atomicku reprezentaciu stavov, ciel je reprezentovany mnozinou cielovych stavov a akcie predstavuju prechody medzi stavmi

- State representations:
  - Atomic: stav je nerozlozitelny (nie je tam ziadne vnutorna struktura, ako cierna skrinka), pouzivane v prehladavacich algoritmoch
 
  - Factored: stav je vektor hodnot (vlastnost sveta - prostredia), pouzivane v CSP, vyrokovej logike, planovani (napr. v CSP mame premenne, ktore maju svoje domains)
 
  - Structured: stav je mnozina objektov (s vlastnymi atributmi) spojene cez rozne relacie, pouzivane v predikatovej logike

# 2. Problem solving and search:

### Formulate a well-defined problem and its (optimal) solution, give some examples.

- Well-defined problem by mat obsahovat:
  - pociatocny stav
  - transition model (prechodovy model):
    - $(state, action)$ -> $state$, model, ktory zoberie aktualny stav, vykona akciu a prejdeme do noveho stavu
  - cielovy (goal) test
 
- Solution (riesenie) nejakeho problemu je postupnost akcii z pociatocneho stavu do cieloveho stavu
- Optimalne riesenie sa snazi minimalizovat hodnotu, ktora ohodnoti kvalitu riesenia (napr. v search tree sa snazime minimalizovat path cost, teda dlzku cesty)

- Priklady:
  1. Search tree:
     - pociatocny stav je koren
     - vetvy su akcie
     - vrcholy su stavy
    
  2. 8-puzzle:
     - pociatocny stav je rozlozenie cisel v mriezke 3x3
     - akcie: posun prazdneho policka hore/dole/doprava/dolava
     - prechodovy model: vymena pozicii
     - ciel: policka zoradene podla cisel 1,2...8,prazdne
     - mozna heuristika: suma cez vsetky policka, kde jedno policko zratame ako Manhattanovsku vzdialenost medzi polickom a jeho spravnym miestom
    
  3. N-Queens
     - pociatocny stav je prazdna doska, kde kralovne su priradene ku kazdemu stlpcu a hladame len riadky
     - stavy su pozicie kralovien na doske
     - cielovy stav je neznamy stav, ale lahko rozpoznatelny, je $N$ kralovien na doske
     - akcia: poloz kralovnu na dosku tak aby nebola v konfliktne s inymi

### Explain and compare tree search and graph search, discuss memory vs time.

- Struktura prehladavacieho algoritmu:
  - dat koren do frontieru (otvoreny list)
  - vybrat vrchol z frontier pomocou nejakej strategie (fronta, zasobnik, prioritna fronta...)
  - expandovat vrchol (pridat naslednikov do frontieru - naslednici su stavy)
  - opakovat dokym fronta nie je prazdna alebo cielovy vrchol (stav) nie je najdeny

- Tree search
  - dokaze explorovat zbytocne cesty, pretoze sa stavy mozu opakovat, nevie identifikovat cykly
  - memory - $O(bm)$, $b$ je vetviaci faktor, $m$ je maximalna hlbka lubovolneho vrcholu
  - time - $O(b^m)$

- Graph search
  - pridame data structure **explored set** (zatvoreny list), ktora si pamata uz expandovane vrcholy, tym sa vyhneme opakovanym stavom
  - memory - $O(b^d)$, $b$ je vetviaci faktor, $d$ je hlbka cieloveho vrcholu
  - time - efektivnejsia, vyhyba sa duplicitam

### Explain and compare node selection strategies (depth-first, breadth-first, uniform-cost).

- DFS:
  - stavy su v zasobniku, LIFO (last in first out) strategia vyberania vrcholu
  - backtracking - jedna vetva je aktualne v pamati,
  - kompletne pre graph-search
  - nekompletne pre tree-search (cykly vedu k nekonecnu)
  - sub-optimal
  - memory $O(bm)$, $b$ je vetviaci faktor, $m$ je maximalna hlbka lubovolneho vrcholu
  - time $O(b^m)$
  
- BFS:
  - stavy su vo fronte, FIFO (first in first out) strategia vyberia vrcholu
  - kompletne (pre konecny vetviaci faktor)
  - optimalne (ak path-cost je neklesajuca)
  - vacsie poziadavky na pamat, musime prejst cele hladiny, teda $O(b^{d+1})$ zlozitost, $b$ je vetviaci faktor, $d$ je hlbka cieloveho vrcholu

- Uninformed-cost (Dijkstra's algorithm):
  - nemame ziadne informacie naviac okrem zadaneho problemu
  - rozsirenie BFSka
  - stavy su v prioritnej fronte podla $g(n)$
  - expandujeme ten vrchol, ktory ma **najnizsiu path cost** $g(n)$
  - goal test je aplikovany na vrchol ked je vybraty na expandovanie miesto toho, ked by bol prvy krat generovany (na zaistenie optimality)
  - novy test ak je najdena lepsia cesta do aktualne prehladavaneho vrcholu

### Informed (heuristic) search: explain evaluation function f and heuristic function h, define admissible and monotonous heuristics and prove their relation. Describe A* algorithm and prove its properties (optimality for tree search and graph search).

- Path cost function $g$
  - $g(n)$ je hodnota cesty od pociatocneho vrcholu do vrcholu $n$

- Evaluation function $f$
  - $f(n)$ je ocakavana hodnota najkratsieho riesenia cez $n$
  - celkovy odhad nakladov cez $n$
  - $f(n) = g(n) + h(n)$ 

- Heuristic function $h$
  - $h(n)$ je odhadovana hodnota najkratsej cesty z vrcholu $n$ do cieloveho stavu (vrcholu)
  - $h(n)$ je lubovolna neklesajuca funkcia, taka, ze ak $n$ je cielovy vrchol, tak $h(n) = 0$

- $h(n)$ je pripustna (admissible) ak:
  - $h(n)$ $<=$ cena najkratsej cesty z $n$ do ciela ("nikdy neprecenuje skutocne naklady do ciela")

- $h(n)$ je monotonna (monotonous)
  - nech $n'$ je naslednik $n$ cez akciu $a$ a nech $c(n,a,n')$ je cena prechodu
  - $h(n) <= c(n,a,n') + h(n')$, ak toto plati, $h(n)$ je monotonna
  - forma trojuholnikovej nerovnosti

- Informed search (A* algorithm):
  - je to identicky algoritmus ako Dijsktra, len tu vieme o probleme viac, a teda vyuzijeme nejaku heuristiku na lepsie ohodnotenie vrcholov (stavov)
  - $f(n) = g(n) + h(n)$
  - optimalny a kompletny (ak $h(n)$ je pripustna/monotonna)
  - Dokazy si pozriet na slidoch

# 3. Constraint satisfaction:

### Define a constraint satisfaction problem (including the notion of constraint) and give some examples of CSPs.

- Constraint satisfaction problem pozostava z:
  - konecnej mnoziny premennych
    - popisuju nejaky stav sveta, ktory hladame (napr. pozicie kralovien na doske)
  - domains (domien) - konecne mnoziny moznych hodnot pre kazdu premennu
    - popisuju moznosti, ktore su dostupne (napr. riadky pre kralovne)
  - konecnej mnoziny obmedzeni (constraints)
    - constraint je relacia nad podmnozinou premennych
    - moze byt definovana ako formula (`rowA != rowB`)

- Dosiahnutelne riesenie CSP je kompletne, konzistentne ohodnotenie vsetkych premennych hodnotami z ich domen, kde:
  - kompletne znamena, ze kazda premenna ma nejaku hodnotu
  - konzistentne znamena, ze vsetky constraints su splnene

- Examples:
  1. N-Queens
    - kazda kralovna je prealokovana na kazdy stlpec, hladame len jej riadok
    - premenne - mame $N$ premien $r(i)$ s domenami ${1,...,N}$
    - constraints - ziadne dve kralovne na seba nemozu utocit: $r(i) != r(j) and |i-j| != |r(i)-r(j)|$ pre vsetky $i$,$j$
 
  2. Sudoku
    - kazde policko je premenna s domenou ${1,...,9}$
    - constraints - pre kazdy riadok, stlpec a 3x3 stvorcek musi platit, ze vsetky hodnoty su rozne

### Apply problem solving techniques to CSPs, explain why a given technique is appropriate for CSPs.

- Backtracking search
  - priradi vybratej premennej (neinstanciovanej) hodnotu
  - skontroluje constraints pre instanciovane premenne
  - ak su contraints splnene, tak pokracuje dalej, inac vyskusa priradit inu hodnotu
  - ak ina hodnota sa neda priradit, tak sa vrati a skusi predoslu hodnotu zmenit
  - opakujeme dokym vsetky premenne nie su instanciovane (a constraints splnene) alebo ak nevyskusame vsetky priradenia
 - tento search sa vyplati robit, pretoze napr. pri N-queens probleme, vieme ze hlbka stromu je $N$ a zaroven kazda vetva vedie do inej mnoziny stavov, teda sa nam vyplati robit tree search s DFS (backtrackingom)

### Explain principles of variable and value ordering and give examples of heuristics.

- backtracking search instanciuje premenne v nejakom poradi, a taktiez priraduje hodnoty v nejakom poradi, to mozeme vyuzit a nejako poradia modifikovat:

- Variable ordering
  - Fail-first principle - snazime sa ohodnotit tie premenne, ktore pravdepodobne zlyhaju v rieseni problemu 

  - Dom heuristic - vybera premennu, ktora ma najmensiu domenu 
  - Deg heuristic - vybera premennu, ktora sa najviac zucastnuje v constrainoch   

- Value ordering
  - Succeed-first principle - hladame hodnotu, ktora patri do riesenia
 
  - napr. hodnota, ktora najmenej obmedzuje ostatne premenne
  - tazke na zratanie

### Define arc consistency and show an algorithm to achieve it (AC-3).

- Arc consistency
  - je to forma localnej konzistencie
  - odstrani hodnoty, ktore porusuju nejake constraint
  - negarantuju globalnu konzistenciu
 
- Arc (hrana medzi dvoma premennymi v constraint network)
  - povieme ze arc $(V_i, V_j)$ je arc konzistentny iff pre kazdu hodnotu $x$ z domeny $D_i$ existuje hodnota $y$ z domeny $D_j$, taka, ze priradenie $V_i = x$ a $V_j = y$ splna vsetky constraints 

- AC-3 algorithm
  - dostane CSP s premennymi ${X_1,...,X_n}$, pouziva frontu, ktora ma v sebe arcs
  - dokym fronta nie je prazdna, vyber prvy arc a skus odstranit nekonzistentne hodnoty - teda tie, kde ak si vyberiem $x$ z $D_i$, tak musi existovat $y$ z $D_j$, inac $x$ odstranim (domain filtering)
  - ak som nejaku hodnotu z domeny odstranil, tak pre kazdeho suseda $X_k$ pre $X_i$ pridaj arc $(X_k, X_i) do fronty$

### Define k-consistency and explain its relation to backtrack-free search; give an example of a global constraint.

- k-consistency
  - kontrola konzistencie, kde pre konzistentne priradenie $k-1$ premennych ziadame konzistentnu hodnotu v este jednej premennej
  - 1-consistent = kazda hodnota v domene splna unarne contraints
  - 2-consistent = kazda dvojica premennych je konzistentna
  - 3-consistent = ak mame priradene hodnoty 2 premennym, existuje hodnota pre tretiu
  - plati, ze ak je problem i-konzistentny pre $i=1,...n$, kde $n$ je pocet premennych, tak ho dokazeme vyriesit pomocou backtrack-free searchu, problem ale je, ze casova zlozitost k-consistency je exponencialne v $k$
 
- globalne constraints
  - globalna constraint enkapsuluje podproblem s specialnou strukturou
  - napr. `all_different({X_1,...,X_k})`
    - enkapsuluje mnoziny nerovnosti $X_1 != X_2$, $X_1 != X_3$,..., $X_{k-1} != X_k$
    - filterovanie domen prebieha podla najvacsieho parovanie (pomocou bipartitnych grafov, premenne na jednej strane a hodnoty domen na druhej strane)
    
### Explain forward checking and look ahead techniques.

- Forward checking
  - backtracking search s lepsim odstranovanim
  - na priklade s N-queens to znamena, ze stale ked priradime kralovnu na dosku, tak z dosky odstranime vsetky konfliktne policka, ktore zatial nie su instanciovane
  - teda vseobecne, v momente, kedy je premennej priradena hodnota odstranime konfliktne, neinstanciovane premenne
  - skontroluje contraints len pre instanciovane premenne

- Look ahead
  - backtracking serach + arc consistency
  - spravime problem arc consistent
  - po kazdom priradeni hodnoty do premennej pocas prehladavania, tak vykoname arc consistency znova
  - skontroluje vsetky constraints, a teda odstrani viac nekonzistentnosti

# 4. Knowledge representation and propositional logic:

### Define a knowledge-based agent.

- Knowledge-based agent
  - agent, ktory pouziva knowledge base na vykonavanie rozhodnuti
  - pouziva logicku inferenciu, aby vyvodil nove fakty a urcil spravnu akciu
 
- Knowledge base (KB)
  - mnozina vyrokov vyjadrenych v danom jazyku
  - vyroky mozu byt updatetnute (pridane) cez operaciu TELL - agent sa nauci, ze vyrok je pravdivy
  - su dopytovane operaciou ASK - agent zistuje, ci vyrok je logickym dosledok KB

### Define a formula in propositional logic, describe conjunctive and disjunctive normal forms (and how to obtain them), define Horn clauses.

- literal je atomicka premenna alebo jej negacia
- klauzula je disjukcia literalov
- formula je validne zlozenie vyrokovych premennych (literalov) a logickych spojok (and, not, or, iff, implies)
- vyrok (formula) je v CNF, ak je konjukciou klauzili
- vyrok (formula) je v DNF, ak je disjukciou konjunkcii literalov
- vyroky mozeme upravovat pomocou logickych uprav, De Morganove pravidla, dokym nedostaneme vyrok v DNF alebo CNF
- Kazda veta vo vyrokovej logike ma svoj ekvivalentny tvar v CNF
- Hornovske klauzule su klauzule, ktore maju najviac jeden pozitivny literal

### Explain the notions of model, entailment, satisfiability, and their relations.

- Povieme ze $M$ je model nejakeho vyroku $a$, ak $a$ je pravdiva v $M$ (kde teda $M$ je ohodnotenie pravdivostnych hodnot vsetkym vyrokovym premennym)

- Splnitelnost
  - povieme ze formula je splnitelna, ak existuje aspon jeden model, v ktorom je pravdiva, inac je nesplnitelna

- Entailment (logicky dosledok)
  - KB |= phi, znamena, ze vyrok phi je logickym dosledkom KB, to plati iba ak $M(KB)$ su podmnozinou $M(phi)$
 
- Vztahy
  - Ak KB |= phi, tak potom KB and not(phi) je nesplnitelna

### Explain DPLL algorithm (including the notions of pure symbol and unit clause).

- DPLL
  - backtrackingovy algoritmus pre kontrolu splnitelnosti CNF formul
  - vrati true, ak kazda klauzula je splnena v modele
  - vrati false, ak nejaka klauzula nie je splnena v modele
  - pozrie sa ci existuje cisty symbol
    - cisty symbol je symbol, ktory ma vo vsetkych jeho vyskytoch zo vsetkych klauzulach rovnake znamienko, teda bud vsade sa vyskytuje ako pozitivny literal, alebo ako negacia 
    - tento literal je potom nastaveny na true
    - vsetky klauzule, ktore obsahuju tento symbol su potom splnene
  - pozrie sa ci existuje jednotkova klauzula
    - jednotkova klauzula je klauzula, ktora obsahuje len jeden symbol (literal)
    - potom je tento symbol nastaveny na true
    - vsetky klauzule, ktore obsahuju tento symbol su potom splnene
    - vsetky klauzula, ktore obsahuje opakcne znamienko tohto symbolu maju tento symbol odstraneny z klauzule
  - rozvetvujeme pre backtracking, kde ohodnotime lubovolny symbol (alebo za pomoci mudrej heurisitiky) na true a potom false
 
- Mozne heuristiky
  - Degree heuristic - vyberie symbol, ktory sa vyskytuje najviac v klauzulach
  - Activity heuristic - vyberie symbol, ktory sa vyskytuje v najviac konfliktoch

### Explain resolution algorithm (and how it proves entailment) and explain forward and backward chaining as its special cases.

- Rezolucia
  - je to metoda dokazovania, zalozena na odvodzovani novej klauzule z dvoch klauzul, ktore obsahuju komplementrane literaly
  - rezolucne pravidlo
    - ak mame napr. klauzule (A or B), (not(A) or C), tak z nich mozeme odvodit (B or C)
  - ked dokazujeme nesplnitelnost, tak pridame negaciu ciela (toho, co chceme dokazat)
  - prevedieme formulu do CNF
  - algoritmus skonci ked
    - uz sa neda vytvorit ziadna nova klauzula (potom not(KB |= phi))
    - sa odvodi prazdna klauzula (potom KB |= phi), teda phi plati

- Forward chaining
  - pracuje s Hornovskymi klauzulami
  - inearny cas vzhladom k velkosti knowledge base
  - z faktov do zaverov
  - data-drive metoda
  - postup:
    - z faktov, ktore pozname si zistime vsetky mozne dosledky cez Hornovske klauzule, dokym uz neexistuju nove fakty, alebo kym nedokazeme otazku

- Backward chaining
  - pracuje s Hornovskymi klauzulami
  - linearny cas vzhladom k velkosti knowledge base
  - z otazok (queries) do faktov
  - goal-driven reasoning
  - otazka je rozdelena (cez Hornovske klauzule) do sub-otazok, dokym nie su ziskane fakty z KB

# 5. Automated planning:

### Define planning domain and problem (representation of states, planning operator vs action, applicability and relevance of action, transition function, regression set).

- Frame problem
  - axiommy v pozadi sa nemenia
  - mozeme pridat frame axioms, ktore explicitne zachovaju stav vyrokov v dalsom kroku 

- Fluents
  - vyrokove premenne, ktorych hodnota sa moze menit medzi stavmi (v case $t$
 
- Rigid predicates
  - nemenne vlastnosti (napr. adjacent(room1, room2))

- Stavy su reprezentovane
  - je to mnozina literalov, ktore popisuju pravdivostnu hodnotu fluents
  - faktorizovana reprezentacia stavov (stav je vektor hodnot)
 
- Operator (action schema)
  - popisuje akciu, bez specifikacie presnych objektov, na ktore je akcia aplikovana
  - operator $o$ je trojica $(name(o), precond(o), effect(o))$
    - $name(o)$ je nazov operatoru vo forme $n(x_1,...,x_k)$, kde $n$ je symbol operatoru (unikatny nazov) a $(x_1,...,x_k)$ su symbolu premennych
    - $precond(o)$ su literaly, ktore musia v stave platit aby bol operator na ne aplikovatelny
    - $effect(o)$ su literaly, ktore sa stanu pravdive (alebo nepravdive) po aplikovani operatora (iba fluents tam mozu byt)
   
- Akcia
  - akcia je plne instanciovany operator (teda, ze za specifikacie typov objektov tam sometne objekty dosadim) 

- Notacia
  - nech $S$ je mnozina literalov
  - $S^+$ = mnozina pozitivnych atomov v $S$
  - $S^-$ = mnozina negacii v $S$

- Akcia $a$ je aplikovatelna na stav $s$ prave ked
  - $precond^+(a)$ je podmnozinou $s$ and $precond^-(a)$ prienik $s$ = prazdna mnozina

- Akcia $a$ je releventna pre ciel $g$ iba ak
  - akcia prispeje ku $g$:
    - $g$ prienik $effects(a)$ != prazdna mnozina
  - efekty akcie nie su v konflikte s $g$:
    - $g^-$ prienik $effects^+(a)$ = prazdna mnozina
    - $g^+$ prienik $effects^-(a)$ = prazdna mnozina

- Prechodovy model
  - vysledok aplikovania akcie $a$ v stave $s$ je
    - $gamma(s,a)$ = $(s-effects^-(a))$ zjednotenie $effects^+(a)$
    - odstranime negativne efekty, pridame pozitivne efekty

- Regression set pre ciel $g$ a relevantnu akciu $a$ je
  - $gamma^{-1}(g,a)$ = $(g-effects(a))$ zjednotenie $precond(a)$
  - odstranime to, co akcia zmeni a pridame preconditions tejto akcie

- Planning domain
  - popis vsetkych moznych akcii agenta, ich preconditions a efekty
  - obsahuje:
    - mnozinu operatorov $O$ (action schemat)
    - popis predikatovych symbolov, objektov

- Planning problem $P$
  - je to trojica $(O, s_0, g)$, kde:
    - $O$ je planning domain model (popis operatorov)
    - $s_0$ je pociatocny stav (mnozina instanciovanych literalov)
    - $g$ je cielova podmienka (mnozina instanciovanych literalov)
   
- Plan je postupnost akcii

- Plan $pi = <a_1,...,a_k>$ je plan riesenie pre problem $P$ iba ak $gamma^*(s_0, pi)$ splna cielovu podmienku $g$

- Priklad
![image](https://github.com/user-attachments/assets/cb1bc3d0-9ccd-41a3-8f4d-00ddce6164ea)

### Explain progression/forward planning and regression/backward planning.

- Forward planning
  - planuje z pociatocneho stavu do cieloveho stavu
  - implementovane prehladavacim algoritmom $A^*$ (prehladavaci algoritmus zaruci nedeterministickost
  - mame prazdny plan, a v loope kontrolujeme:
    - ak $s$ splna $g$, vrat plan
    - mnozina $E$ je mnozina akcii, kde akcia je 
    - ak $E$ je prazdna mnozina, vrat failure
    - nedeterministicky vyber akciu $a$ z mnoziny $E$
    - prejdi do noveho stavu po vykonani akcie $a$ v aktualnom stave
    - pridaj akciu do planu, teda plan = plan,a

- Backward planning
  - planovanie zacina s cielom a aplikuje akcie naspat dokym sa nedostane do pociatocneho stavu
  - iba relevantne akcie su prehladavane
  - zacne s prazdnym planom a potom v loope opakujeme:
    - ak $s_0$ splna $g$, vrat plan
    - mnozina $A$ je mnozina akcii,
    - ak $A$ je prazdna mnozina, vrat failure
    - nedeterministicky vyber akciu z $A$
    - pridaj ju do planu (pred plan), teda plan = a,plan
    - aktualizuj ciel $g$ na $gamma^-1(g,a)$, co je vlastne regression set

### Describe how planning can be realized by logical reasoning (situation calculus).

- Stavy
  - su reprezentovane cez vztahy medzi objektmi (napr. at(robot, location))

# 6. Knowledge representation and probabilistic reasoning:

- Probabilistic agent
  - na rozdiel od logickeho agenta, ktory tvrdi, ze veta je bud pravdiva alebo nepravdiva, pravdepodobnostny agent priraduje mieru viery cislom medzi 0 a 1
  - miera viery je interpretovana ako pravdepodobnost, ze vyrok je pravdivy, vzhladom na znema informacie

### Define the core notions (event, random variable, conditional probability, full joint probability distribution, independence).

Pravdepodobnostne tvrdenia maju mozne svety - **sample space**. Kazdy mozny svet omega je asociovany s pravdepodobnostnou hodnotou $P(omega)$ taka, ze $0 <= P(omega) <= 1$ a $\sum_{mozne svety omega} P(omega)=1$

- Event
  - mnoziny moznych svetov
  - pravdapodobnost nejakeho eventu je suma pravdepodobnosti moznych svetov v evente (napr. $P(event) = P(omega_1, omega_2, omega_3)$, pre $event = { omega_1, omega_2, omega_3 }$
  - pouzivame faktorizovanu reprezentaciu na reprezentaciu svetov, mozny svet je teda reprezentovany mnozinou premien (nahodne premenne) 

- Nahodna premenna
  - premenne v pravdepodobnosti sa nazyvaju nahodne premenne
  - kazda nahodna premenna ma svoju domenu - mnozinu moznych hodnot, ktore moze mat (napr. cavity {true,false}, kocka {1,...,6})

- Podmienena pravdepodobnost
  - $P(a | b) = \frac{P(a and b)}{P(b)}$, kde $P(b) > 0$
  - zaujima nas event $a$, po tom, co nastal event $b$, $b$ sa typicky nazyva evidenciou

- Full joint probability distribution
  - tabulka vsetkych kombinacii hodnot nahodnych premennych a ich pravdepodobnosti

- Nezavislost
  - uplna nezavislost - $P(X,Y) = P(X)P(Y)$ alebo $P(X | Y) = P(X)$ alebo $P(X | Y) = P(Y)$
  - podmienena nezavislost
    - $P(X | Y,Z) = P(X | Z)$ alebo $P(Y | X,Z) = P(Y | Z)$ alebo $P(X, Y | Z) = P(X|Z)P(Y|Z)$, nahodne premenne $X, Y$ su podmienene nezavisle pre danu nahodnu premennu $Z$
    - priklad: $P(Catch, Toothache | Cavity) = P(Catch | Cavity)P(Toothache | Cavity)$ 
      
### Explain probabilistic inference (Bayes’rule, marginalization, normalization, causal direction, diagnostic direction).

- Marginalizacia (summing out)
  - ak chceme zistit $P(X)$, pricom mame full disjoint probability distribution nad $X, Y$, mozeme marginalizovat premennu $Y$:
    - $P(X) = \sum_y P(X,y)$

- Normalizacia
  - Ked pocitame podmienenu pravdepodobnost
    - $P(X | e) = alfa P(X, e)$, kde alfa = $\frac{1}{P(e)}$ je normalizacna konstanta, ktora zabezpeci, ze vysledne pravdepodobnosti sa scitaju na 1 

- Bayesovo pravidlo
  - $P(cause | effect) = \frac{P(effect | cause) P(cause)}{P(effect)}$

- Casual direction
  - $P(effect | cause)$

- Diagnostic direction
  - $P(cause | effect)$

### Define Bayesian network, explain its relation to full joint probability distribution; describe a method for constructing Bayesian networks (explain chain rule); describe inference techniques for Bayesian networks (enumeration, variable elimination, rejection sampling, likelihood weighting).

- Bayeska siet
  - Grafova reprezentacia pravdepodobnosti medzi premennymi
  - Ide o acyklicky orientovany graf (DAG)
    - Nodes - mnozina nahodnych premennych
    - Hrany - zavislosti medzi premennymi
    - Kazdy uzol ma podmienenu pravdepodobnost vzhladom na svojich rodicov
   
  - reprezentuje full joint probability distribution
    - $P(x_1,...,x_n) = ∏_i P(x_i | parents(X_i))$

- Konstrukcia Bayeskej siete
  -

  - chain rule - 

# 7. Probabilistic reasoning over time:

### Define transition and observation models and explain their assumptions.

### Define basic inference tasks (filtering, prediction, smoothing, most likely explanation) and show how they are solved.

### Define and compare Hidden Markov Model and Dynamic Bayesian Network.

# 8. Decision making:

### Formalize rationality via maximum expected utility principle (define expected utility and describe relation between utility and preferences).

### Define decision networks and show how the rational decision is done.

### Define a sequential decision problem (Markov Decision Process and its assumptions) and its solution (policy); describe Bellman equation and techniques to solve MDP (value and policy iteration); formulate Partially Observable MDP and show how to solve it.

# 9. Adversarial search and games:

### Explain core properties of environment and information needed to apply adversarial search.

### Explain and compare mini-max and alpha-beta search.

### Define an evaluation function and give some examples.

### Describe how stochastic games are handled (expected mini-max).

### Define single-move games, explain the notions of strategy, Nash equilibrium, Pareto dominance, explain Prisoner’s dilemma; define maximin technique and show some strategies for repeated games.

### Mechanism design: explain classical auctions (English, Dutch, sealed bid) and tragedy of commons (and how it can be solved).

# 10. Machine learning:

### Define and compare types of learning (supervised, unsupervised, reinforcement), explain Ockham’s Razor principle and diYerence between classification and regression.

### Define decision trees and show how to lean them (including the definition of entropy and information gain).

### Describe principles and methods for learning logical models (explain false negative and false positive notions, describe current-best-hypothesis and version-space learning).

### Describe linear regression, show its relation to linear classification (define linearly separable examples) and artificial neural networks (describe backpropagation technique).

### Explain parametric and non-parametric models, describe k-nearest neighbor methods and the core principles of Support Vector Machines (maximum margin separator, kernel function, support vector).

### Describe and compare Bayesian learning and maximum-likelihood learning; describe parameter learning for Bayesian networks including expectationmaximization (EM) algorithm (explain the notion of hidden variable).

### Formulate reinforcement learning problem, describe and compare passive and active learning; describe and compare methods of direct utility estimation, adaptive dynamic programming (ADP), and temporal diYerence (TD) for passive learning; explain the diYerence between model-based and model-free approaches; explain active version of ADP and the notions of greedy agent and exploration vs exploitation problem; describe exploration policies (random vs exploration function); describe and compare methods Q-learning (including Qvalue and its relation to utility) and SARSA.
