## 1. Základy strojového učení
------------------------------

### Vysvětlit rozdíly mezi jednotlivými typy strojového učení (učení s učitelem, bez učitele, zpětnovazební učení)
  - **učenie s učiteľom** - sú zadané objekty a k nim príslušné výstupy. Cieľom je nájsť model, ktorý, ak mu je zadaný nejaký objekt, správne priradí správny výstup.
      
  - **učenie bez učiteľa** - sú zadané objekty bez požadovaných výstupov. Cieľom je sa naučiť, ako vstupné objekty vyzerajú a generovať nové objekty (generatívne modely), alebo napr. rozdeliť objekty do skupín obsahujúcich podobné objekty (zhlukovanie).
  
  - **zpětnovazebné učenie** - snažíme sa naučiť chovanie agenta, aby čo najlepšie riešil zadaný problém na základe zpätnej väzby od prostredia, v ktorom sa pohybuje. Trial-and-error učenie, ohodnocujeme jeho akcie a na základe nich agenta penalizujeme alebo pochválime.  

### Vysvětlit rozdíly mezi jednotlivými úlohami strojového učení s učitelem (regrese, klasifikace)
  - Pri učení s učiteľom môžu byť výstupy dvoch typov, buď kategorické, alebo číselné. Ak by šlo o kategorické, tak je cieľom priradiť objekt do správnej kategórie (napr. rozhodnúť, či na obrázku je pes alebo mačka) a problém by sa nazýval klasifikáciou. Ak by šlo o číselné, tak je cieľom na základe vstupných dat predpovedať číselnú hodnotu (napr. cenu nemovitosti na základe informácii o nej), takýto problém sa nazýva regresia. 

### Popsat typy příznaků (kategorické, ordinální, číselné) a jejich předzpracování (kódování kategorických a ordinálních příznaků, škálování číselných příznaků)
  - **kategorické** - reprezentujú kategórie bez nejakého poradia (napr. Farba auta: červená, modrá...). Predspracovanie kategorických príznakov môže byť napr. One-Hot encoding - čo vlastne znamená, že pre každý stav kategórie si vytvoríme binárny znak, alebo Label encoding - každému stavu kategórie priraď unikátne číslo
    
  - **ordinálne** - ide vlastne o kategorické príznaky s poradím, napr. Hodnotenie: nízka kvalita < priemerná kvalita < vysoká kvalita. Predspracovanie ordinálnych príznakov môže byť napr. Ordinálne - priraď každej kategórii číselnú hodnotu.
      
  - **číselné** - ide o kvantitatívne hodnoty s číselným významom, delíme ich na **diskrétne** - počítateľné hodnoty (0, 1, 2..) a **spojité** - nekonečný počet hodnôt v intervale (170.2cm...). Predspracovanie číselných príznakov vyžaduje škálovanie, príkladz škálovaní môžu byť napr. Min-Max Scaling (normalizácia do intervalu [0,1]) alebo napríklad Standard Scaling.

-------------------------
## 2. Zpětnovazební učení
-------------------------

### Popsat problém zpětnovazebního učení intuitivně (agent, prostředí, hledání strategie)
  - je to spôsob učenia, kde sa agent učí optimálneho chovania v svojom prostredí pomocou trail-and-error metódy
  - agent pozoruje stav prostredia, vykoná akciu, dostane zpätnú väzbu (odmenu) a na základe nej si upraví svoje budúce chovanie
  - cieľom je nájsť stratégiu, ktorá maximalizuje celkovú odmenu
  - agent - entita/objekt, ktorý sa učí cez interakcie s prostredím
  - prostredie - všetko, s čím agent dokáže interagovať, obsahuje informácie o stave
  - stav - popis aktuálnej situácie v prostredí, ktorú agent popisuje
  - akcia - rozhodnutie agenta, ktoré v nejakom stave vykoná
  - odmena - číselná hodnota poskytovaná prostredím ako zpätná väzba za prevedenie nejakej akcie
### Popsat prostředí jako Markovský rozhodovací proces
  - prostredie je nejaká zadaná štvorica **$(S,A,P,R)$**, kde:
    - **$S$** je konečná množina stavov prostredia
      
    - **$A$** je konečná množina akcií

    - **$P_a(s,s')$** je prechodová funkcia, ktorá udáva pravdepodobnosť, že aplikácia akcie $a$ v stave $s$ prejde do prostredia v stave $s'$

    - **$R_a(s,s')$** je funkcia odmien, ktorá udáva okamžitú odmenu, ktorú agent dostane od prostredia, ak je v stave $s$ a prevedie akciu $a$, kde nezáleží na predchádzajúcich stavoch a akciách - **Markovská vlastnosť**!!!

  - **Markovská vlastnosť** - budúcnosť závisí iba od aktuálneho stavu a akcie, na predchádzajúcich vôbec nezáleží
  
  - chovanie agenta sa potom da popísať ako strategia (policy) **$π:S×A→[0,1]$**, kde $π(s,a)$ udáva pravdepodobnosť prevedenia akcie $a$ v stave $s$

### Definovat zpětnovazební učení jako problém hledání strategie maximalizující celkovou odměnu agenta
  - cieľom reinforcement learningu je potom najsť strategiu $π$ takú, že maximalizuje celkovú odmenu, ktorú agent získa **$∑_{t=0...∞} γ^t R_{a_t}(st,st+1)$**, kde **$a_t = π(st)$** je akce provedená agentem v kroku $t$ a $γ < 1$ je diskontní faktor, který zajišťuje, že suma je konečná.

### Definovat pojmy hodnota stavu a hodnota akce
  - **hodnota stavu** - hodnota $V^π(s)$ stavu $s$ pri použití strategie $π$ sa dá definovať ako $V^π(s) = E[R] = R[∑_{t=0...∞} γ^t r_t|s_0 = s]$, kde $R$ značí celkovú získanú diskontovanú odmenu a $r_t$ značí odmenu obdržanú v čase $t$.
  
  - **hodnota akce** - hodnota $Q^π(s,a)$ akcie $a$ prevededná v stave $s$ ak budeme sledovať strategiu $π$.  Dá sa to definovať ako $Q^π(s,a) = E[∑_{t=0...∞} γ^t r_t|s_0 = s,a_0 = a]$

### Popsat základní myšlenku Monte-Carlo metod (v rozsahu podle textu na webu)
  - Zpětnovazební učení si lze představit ve dvou krocích - zlepšení strategie a vyhodnocení strategie.
    
  - Monte-Carlo metody sa používajú pre vyhodnotenie strategie (odhad hodnoty funkcí 𝑉(𝑠) a 𝑄(𝑠,𝑎)):
    - Agent v stave $s$ vykoná akciu $a$ podle strategie $𝜋$ dokým sa nedostane do nejakého cieľa (vykonáva teda nejakú sekvenciu akcí - epizódu akcií).

    - Obdrženou odměnu potom zaznamenáme, na konci epizódy.

    - Vylepšení strategie sa potom robí tak, že se spočítá nová (hladová) stratégia na základe práve nových hodnôt $Q(s,a)$.
  
  - Hlavným problémom Monte-Carlo metod je, že ak sú rozptyly odmien pre akcie a stavy veľké tak bude konvergovať veľmi pomaly.

### Vysvětlit Q-učení (včetně vzorce pro aktualizaci matice Q)
  - je to off-policy metoda reinforcement learningu (nezavisí na žiadnej konkrétnej strategie, ktorú agent sleduje), ktorá nepotrebuje poznať model prostredia.

  - algoritmus je založený na temporal-difference (TD) metódach.
  
  - učí sa priamo funkciou $Q(s,a)$, v tradičných prípadoch je $Q$ reprezentovaná ako matica, na začiatku inicializovaná samými nulami. Následne v každom kroku agent pozoruje stav $s_t$ a prevedie akciu $a_t$, dostane odmenu $r_t$ a pozoruje nový stav $s_{t+1}$. Na základe týchto informácií sa maticu upraví následovne: $Q^{new}(s_t, a_t) ← (1 - α)Q(s_t, a_t) + α(r_t + γ max_a Q(s_{t+1}, a))$, kde $α$ je parameter učenia a akcia $a_t$ môže byť vybratá ľubovoľnou metodou. 
  
  - výhoda oproti Monte Carlo metódam je, že sa pri jednom ohodnotení upraví hodnota viacerých stavov.

### Popsat algoritmus SARSA (State-Action-Reward-State-Action)
  - obdobný Q-učeniu, ale výpočet Q sa prevádza podľa vzťahu: $Q(s_t, a_t) ← Q(s_t, a_t) + α[r_t + γ Q(s_{t+1}) - Q(s_t, a_t)]$, kde $a_t$ a $a_{t+1}$ sú založené na strategii používanej agentom, teda ide o on-policy algoritmus.

### Vysvětlit, jak použít Q-učení v prostorech se spojitými stavy a akcemi
  - problém implementácie Q-učenia cez maticu je ten, že funguje len na diskrétných priestoroch a s diskrétnými akciami, teda v prípade spojitých stavov a akcií sa dá postupovať následovne:
      
      - **stavy a akcie diskretizujeme** - napr. na Mountain Car môžeme hodnoty polohy a rýchlosti rozdeliť do intervalov a stav reprezentovať intervalom, miesto konkrétnej hodnoty. V prípade akcíí to budeme riešiť podobne.
  
  - ďalším problémom môže byť veľkosť matice pre stavové priestory, potom sa algoritmus učí veľmi pomaly, alebo vôbec.
      
      - dá sa to vyriešiť tak, že použijeme iné metódy, ktoré sa učia priamo mapovanie zo stavu na akciu, s tými následne spojité akcie problém vačšinou nemajú a taktiež ani veľkosť stavu.

-------------------------------------
## 3. Jednoduchý genetický algoritmus
-------------------------------------

#### Princip
  - Jednoduchý genetický algoritmus funguje na princípe selekcie najlepších jedincov, ktorí sa ďalej rozmnožujú pomocou genetických operátorov, čím vznikajú nové generácie jedincov lepšie adaptovaných na daný problém.

### Popsat základní verzi evolučního algoritmu s binárním kódováním
  - Jednoduchý genetický algoritmus používa reprezentáciu jedinca ako binárny reťazec pevnej dĺžky. Každý bit predstavuje jednu vlastnosť riešenia.

  - Postup algoritmu:
    - Inicializácia populácie – populácia sa generuje náhodne.

    - Výpočet fitness funkcie – hodnotí sa kvalita každého jedinca.

    - Selekcia rodičov – jedinci sa vyberajú podľa určitej stratégie (pomocou ruletovej alebo turnajovej selekcie)  jedinci môžu byť vybraní opakovane. 

    - Aplikácia genetických operátorov na vybraných rodičov (jedincov):
        - Křížení (crossover) – kombinovanie dvoch rodičov na vytvorenie nových jedincov.
        
        - Mutácia – náhodná zmena niektorých bitov na zabezpečenie diverzity, aplikuje sa po krížení

    - Tvorba novej generácie – nový potomkovia nahradia predchádzajúcu populáciu

    - Opakovanie cyklu – algoritmus pokračuje, kým nie je splnené kritérium zastavenia (napr. maximálny počet iterácií alebo konvergencia na optimálne riešenie).
   
### Popsat základní genetické operátory (jednobodové, n-bodové křížení, mutace)
  - **jednobodové kríženie** - náhodne sa vyberie jeden bod v jedincovi a nový potomok vznikne tak, že sa skopíruje čásť z jedneho rodiča pred týmto bodom a z druhého rodiča od toho bodu ďalej.
  
  - **n-bodové kríženie** - podobne ako jednobodové, ale existuje viacero bodov zmeny. Potomci teda vznikajú tak, že sa alternuje, z akého rodiča je vybratá aká čásť.

  - **uniformné kríženie** - u každej pozície sa znova náhodne rozhodneme, z ktorého rodiča dosadíme hodnotu do potomka.
  
  - **mutácia** - náhodná zmena niektorých bitov na zabezpečenie diverzity, v prípade binárneho kódovania ide o preklopenie bitov.
 
### Popsat ruletovou a turnajovou selekci
  - **ruletová selekcia** - pravděpodobnost výběru daného jedince je přímo úměrná jeho fitness. Formálne pravdepodobnosť $p_i$, že si alg. vyberie jedinca $i$ sa spočíta ako $p_i = \frac{f_t}{∑_{j=1...N}f_j}$, kde $f_j$ je fitness jedinca $j$
  
  - **turnajová selekcia** - z populácie sa náhodne vyberie podmnožina jedincov (typicky 2) a najlepší z nich (porovnaním ich fitness) s veľkou pravdepodobnosťou (napr. 80%) postúpi do ďalšej generácie.
    
### Vysvětlit výhody a nevýhody turnajové a ruletové selekce
  - výhody:

    - ruletová selekcia: jednoduchá implementácia, funguje fajn pre rozumný rozsah fitness hodnôt

    - turnajová selekcia: funguje aj pri záporných fitness hodnotách. Záleží len na poradí jedincov podľa fitness.
  
  - nevýhody:

    - ruletová selekcia: záleží priamo na hodnotách fitness, teda ak fitness zmeníme tak, že k nej pripočítame nejakú náhodnú konštantu, tak sa začne chovať náhodne.

    - turnajová selekcia: pri zlom nastavení veľkosti turnaja môže prísť k rýchlej konvergencii na nie optímalne riešenie.

### Vysvětlit, co je fitness funkce
  - je to hodnotiaca funkcia, ktora kazdemu jedincovi v populacii priradi ciselnu hodnotu podla kvality jeho riesenie daneho problemu. Typicky sa tato fitness funkcia maximalizuje, teda cim vacsia hodnota, tym lepsi jedinec, ale existuju aj minimalizacne problemy.

### Popsat elitismus
  - elitismus znamená, že sa najlepší jedinci automaticky prenesú do ďalšej generácie bez aplikácie genetických oprátorov, čím sa vlastne nestratia najlepšie riešenia. Následne je potom potreba už len vybrať toľko potomkov, aby sa populácia naplnila, pretože sa v jednoduchom genetickom algoritme počet rodičov a potomkov musí rovnať (až na daľši bod).
    
### Popsat metody pro environmentální selekci (přenos jedinců do další populace), (μ,λ) a (μ+λ)
  - (μ,λ): do novej populacie sa vybera μ najlepsich jedincov len spomedzi λ potomkov, rodicia su uplne odstraneni. Potrebne aby λ > μ.

  - (μ+λ): do novej populacie sa vybera μ najlepsich jedincov spomedzi μ rodicov a λ potomkov. Zarucuje elitismus, najlepsi jedinec nemoze byt prehliadnuty.

### Popsat rozšíření jednoduchého GA na problémy s celočíselným kódováním

  - Typicky len miesto vektoru 0 a 1-tiek pouzijeme vektor celych cisel o pevnej dlzke ako reprezentaciu jedinca

  - Operatory:
    - Krizenie: moze ostat rovnake, jednobodove, n-bodove, uniformne nad celociselnymi vektormi
    - Mutacie: napr. nahodne zmenine nejaku hodnotu na inu, ale k nej priratame/odcitame nejaku konstantu

--------------------------------------------------
## 4. Evoluční algoritmy pro spojitou optimalizaci
--------------------------------------------------

### Definovat problém spojité optimalizace

  - Problem spociva v hladani globalneho extremu (typicky minima) realnej funkcie f:$R^n$ -> $R$.

  - Hladame teda vektor $x$, pre ktory plati: $x^* = arg min f(x)$ z definicneho oboru $D$.

  - Typicke pri ladeni parametrov, trenovani modelov alebo optimalizacii technologickych procesov.

### Popsat genetické operátory používané ve spojité optimalizaci - aritmetické křížení, zatížené a nezatížené mutace

  - Mutacia:
    - Nezatizena: Nahradenie hodnoty na vybranej pozicii novou nahodnou hodnotou z nejakeho intervalu
      
    - Zatizena: Nova hodnota vychadza z povodnej hodnoty (napr. Gaussovska mutacia - k danemu cislu pricita hodntou z normalneho rozdelenia so strednou hodnotu 0 a nejakym rozptylom)
   
  - Krizenie:
    - Mozeme pouzit klasicke n-bodove krizenie

    - Aritmeticke krizenie: potomok sa ziska ako vazeny priemer rodicov. 

### Vysvětlit rozdíl mezi separabilními a neseparabilními funkcemi

  - Separabilne funkcie: Dokazu sa optimalizovat nezavisle po jednotlivych zlozkach. To znamena ze pre kazdu premennu zvlast dokazeme najst jej optimum

  - Neseparabilne funkcie: Medzi premennymi existuju interakcie, zmena jednej premennej ovplvni zavislost na ostatnich.

### Popsat algoritmus diferenciální evoluce a jeho výhody

  - Je to evolucny algoritmus pre spojitu optimalizaciu

  - Mutacia: k jedincovi pricita rozdiel dvoch nahodnych jedincov z populacie

  - Krizenie Uniformne krizenie s dalsim nahodnym jedincom

  - Selekcia: Potomok sa porovna s rodicom a v populacii sa necha lepsi z nich

  - Vyhody: Invariantnost voci rotaciam a skalovaniu priestoru, nevadia jej neseparabilne funkcie, lahka implementacia

---------------------------------------------------------
## 5. Evoluční algoritmy pro kombinatorickou optimalizaci
---------------------------------------------------------

### Popsat mutace pro permutační kódování

  - Musi sa zachovat permutacia, teda napriklad mozne mutacie mozu byt:
    - Swapping: Prehodenie dvoch nahodne vybranych pozicii
    - Inversion: Nahodne otocime usek permutacie
    - Insertion: Zoberieme prvok z nejakej pozicie a vlozime ho na inu

### Popsat křížení používaná při permutačním kódování (OX, PMX, ER)

  - OX (Order crossover):
    - Zvolia sa dva body
    - Strednne casti sa zamenia
    - Zbytok je doplneny v rovnakom poradi ako v druhom rodicovi, ale vynechaju sa duplicity (zaciname od druheho bodu)
   
    - 12|345|678      ..|186|...      45|186|723 (přeskakujeme čísla 186)  
    - 34|186|527      ..|345|...      86|345|271 (přeskakujeme čísla 345)

  - PMX (Partially Mapped crossover):
    - Zvolia sa dva body
    - Stredne casti sa zamenia
    - Hodnoty, ktore neboli zamenene v strede ostanu sa svojich poziciach
    - Ak vzniknu duplicity, nahradime ich podla pozicii hodnot v strednych castiach (zo zamenenho stredu na povodny stred)
   
    - 12|345|678      .2|186|.7.      32|186|574 (1->3, 6->5, 8->4)
    - 34|186|527      ..|345|.27      18|345|627 (3->1, 4->8, 5->6) 

  - ER (Edge recombination):
    - Funguje specialne pre problem obchodneho cestujuceho
    - Zostavi sa zoznam susedov kazdeho vrcholu z oboch rodicov
    - Zacneme vrcholov s najmensim poctom susedov
    - Iterativne sa vyberie dalsi vrchol s najkratsim zoznamom susedov
    - Susedov aktualizujeme tak, ze odoberieme tych, ktori uz su pouziti.

### Vysvětlit použití evolučních algoritmů pro problém obchodního cestujícího

  - Problem hlada najkratsi cyklus cez vsetky vrcholy uplneho grafu
  - Jedinec je reprezentovany ako permutacia vrcholov
  - Permutacia hovori v akom poradi ma jedinec jednotlive vrcholy navstivit

-------------------------------------------------
## 6. Lineární a kartézské GP, gramatická evoluce
-------------------------------------------------

- Geneticke programovanie: technika, ktora umoznuje pomocou evolucnych algoritmov automatiky vytvarat programy

### Popsat lineární genetické programování (kódování jedince, operátory)

- Jedinec je reprezentovany ako postupnostou intrukcii

- Kazda instrukcia moze manipulovat s registrami, datovym polom, vstupmi a vystupmi (priklad: `input / 0 / save / input / add / output`)

- Geneticke operatory:
  - Mutacia: nahradime jednu instrukciu inou nahodne zvolenou
 
  - Krizenie: skombinujeme casti instrukcii z dvoch rodicov - napr. 1-bodovy crossover

### Popsat kartézské genetické programování (kódování jedince, operátory)

- Program je zadany na mriezke $r x l$, ktora ma $r$ riadkov a $l$ vrstiev (stlpcov)

- Jedinec je vektor $r x l$ genov

- Gen sa sklada z dvoch casti - kodu (mena) funckie, ktoru pocita a indexov do predchadzajucich vrstiev, z ktorych berie vstupy

- Geneticke operatory:
  - Mutacia:
      - Zmena funkcie na inu
      - Zmena vstupov funkcie
      - Zmena vystupov programu
 
  - Krizenie: obvykle sa nepouziva

### Popsat gramatickou evoluci (kódování jedince, operátory)

- Jedinec je postupnost celych cisel, ktora urcuje, ktore pravidlo z gramatiky sa ma pouzit pre prepis aktualneho prveho neterminalu (cez operaciu `index % pocet_pravidiel`)

- Pouziva sa bezkontextova gramatika ako syntax programovacieho jazyka

- Geneticke operatory:
  - Mutacia: Zmena niektorych cisel v postupnosti cisel
 
  - Krizenie:
    - Klasicke n-bodove krizenie
    - Krizenie, ktore vyberie z kazdeho jedinca gen, kde sa zacina zpracovavat nejaky neterminal a prehodi ho s poziciou v druhom jedincovi, kde sa objavi ten isty neterminal. Vyhoda: zachovanie semantickosti a stale mame validneho potomka

### Vysvětlit, jak řešit problém s příliš krátkým nebo dlouhým jedincem v gramatické evoluci

- Ak je jedinec prilis kratky - tak nemusi byt vyhodnotitelny, mozu v nom ostat neterminaly:
  - Ako riesenie sa snazime generovat dlhsich jedincov, ak jedinec nepouzije niektore svoje geny nie, tak to az tak nevadi.

- Ak je jedinec prilis dlhy - tak mame zbytocne geny naviac:
  - Ako riesenie mozeme geny ignorovat, alebo ich pripadne zmazat

-------------------------------------
## 7. Stromové genetické programování
-------------------------------------

### Popsat stromové genetické programování a jeho genetické operátory

- Reprezentacia jedinca
  - Jedinec je strom
  - Uzly - funkcie (napr. +, *, if, and, or,...) - neterminaly
  - Listy - terminaly (napr. vstupy, konstanty)

- Vyhodnotovanie prebieha rekurzivne od listov ku korenu

- Hodnota korena je vystup programu

- Geneticke operatory:
  - Mutacia:
    - Zmena funkcie/terminalu na iny
    - Nahradenie podstrou novym nahodnym
    - Redukcia podstromu na terminal
 
  - Krizenie: Vymena podstromov medzi dvoma jedincami

### Popsat práci s konstantami ve stromovém genetickém programování

- Nie je mozne dat vsetky cisla medzi terminaly, bolo by ich vela

- Casto sa zadaju len tie zakladne konstanty (0,1,2, apod.) a predpoklada sa, ze dalsie hodnoty si program dokaze spocitat sam z nich

- Druha moznost je mat specialne opratory, ktore menia hodnoty konstant podobne ako v spojitej optimalizacii

### Popsat typované genetické programování

- Kazda funkcia ma presne definovane typy vstupov a vystupov, napr. `add: int x int -> int`

- Potom sa stromy generuju a upravuju tak, aby boli typovo korektne

- Vyhoda: Zabranime typ vzniku nevalidnych stromov, vacsia prehladnost

### Popsat automaticky definované funkce

- Geneticke programovanie moze vytvarat a pouzivat vlastne funkcie (subprogramy)

- Tieto funkcie su ulozene v oddelenej casti jedinca

- Hlavny program (strom) moze volat automaticky definovane funkcia ako bezne funkcie

- Kazda automaticky definovana funkcia ma svoj vlastny strom, omedzene vstupy a definovane telo

### Vysvětlit, jak u automaticky definovaných funkcí zabránit zacyklení/nekonečné rekurzi

- Zakazat rekurzivne volania automaticky definovanych funkcii

- Omedzit hlbku volania funkcii

### Porovnat jednotlivé typy genetického programování a jejich výhody/nevýhody

- Linearne GP:
  - Vyhody: Blizko realnym programom, jednoduche operatory, jazyk
  - Nevyhody: Mensia flexibilita

- Kartezske GP:
  - Vyhody: Prehladna struktura
  - Nevyhody: Tazsie krizenie - nepouziva sa

- Gramaticka evolucia:
  - Vyhody: Zarucena syntakticka spravnost
  - Nevyhody: Problem s dlzkou jedinca (prilis kratky/dlhy)

- Stromove GP:
  - Vyhody: Lahka vizualizacia, garantovana korektnost ak pouzivame typovanost, prirodzenost, lahko vypocitatelne
  - Nevyhody: Typovanie, Ako reprezentovat pociatocnu populaciu (dany pocet neterminalov/v danej hlbke vzdy pouzit terminal...), riziko zacyklenia ak pouzijeme automaticky definovane funkcie

----------------------------------
## 8. Perceptronové neuronové sítě
----------------------------------

- pouzivaju sa casto pre klasifikaciu alebo regresiu v strojovom uceni

### Popsat, jakým způsobem počítá jeden perceptron

- zakladna jednotka v neuronovej sieti

- perceptron je jednoduchy model umeleho neuronu, ktory prijima vstupny vektor $x = (x_1,...,x_n)$, priradene vahy $w = (w_1,...,w_n)$ a pocita jeden vystup $y$ podla:
  - $ξ = \sum_{i = 1}^n w_i x_i + b$
  - $y = f(ξ)$
  - kde $b$ je bias, ktory ovplyvnuje, nakolko musi byt vazeny sucet vacsi ako 0, aby sa perceptron aktivoval a $f$ je aktivacna funkcia
 
  - taktiez druha moznost je pridat bias ako dalsi vstup s konstantnou hodnotou 1, vypocet perceptronu potom vyzera takto: $f(\sum_{i=0}^n w_i f_i)$, kde $f$ je funkcia, ktora pre $x<0$ vrati 0, inac 1.
 
  - taktiez existuje funkcia, ktora miesto 0 a 1 vracia -1 a 1, rozdiel je len vo funkcii.

### Popsat algoritmus pro trénování perceptronu a vysvětlit, kdy konverguje

- perceptrony trenujeme tak, ze postupne sa predkladaju vstupy (x,y), z trenovacej mnoziny, kde x je vstup a y je pozadovany vystup a:
  - vahy aktualizujeme: $w_i$ <- $w_i + r(y-f(x))x_i$
  - kde $r$ je learning rate, ako rychlo sa meni vahovy vektor

- perceptron konverguje, ak su triedy v datach linearne separabilne, teda z nich dokazeme oddelit nadrovinu v vstupnom priestore

### Popsat strukturu vícevrstvých perceptronových sítí

- struktura sa sklada z:
  - vstupnej vrstvy - vstupne data su priamo z trenovacej mnoziny
  - jednej alebo viacerych skrytych vrstiev - vstupy su vystupy z predoslej vrstvy
  - vystupnej vrstvy - neurony produkujuce vystup, vstupy su vystupy z predoslej vrstvy
 
- spojenie je dopredne (feedforward), ziadne cykly, tie su az v rekurentnych neuronovych sietach

### Popsat aktivační funkce používané ve vícevrstvých perceptronech (sigmoida, tanh, ReLU)

- sigmoida:
  - pouzivaju sa najma v starsich sietach
 
  - $f(x) = \frac{1}{1 + e^{-λx}}$

- tanh:
  - vystup [-1,1], symetricka okolo nuly, casto sa povazuje za vhodnejsiu ako sigmoida
 
  - $f(x) = tanh(x) = \frac{e^x - e^{-x}}{e^x + e^{-x}}$

- ReLU (Rectified Linear Unit):
  - vyhodou je jednoduchy vypocet
 
  - $f(x) = max(0, x)$

### Popsat metodu pro trénování neuronových sítí (backpropagation)

- ciel je minimalizovat chybovu funkciu $L(x,y|w) (napr. MSE - mean squared error)

- backpropagation funguje tak, ze:
  - najprv postupuje ako feedforward siet, kedy ziskame nejaky vystup k danym vstupom
  - potom si zratame tzv. loss function, ktora je rozdielom aktualneho vystupu od ocakavaneho
  - potom sa vraciame spat a snazime sa opravit vahy medzi neuronmi pomocou gradient descend algoritmu tak, aby vystup uz bol spravny
 
- gradient descend algoritmus   

  - dopredny vypocet: zrata vystupy vsetkych vrstiev
  - vypocet chyby vystupu: porovna vystup siete s cielom
  - zpetny prechod: spocitaj gradienty podla retazoveho pravidla
  - aktualizacia vah: $w_ij^{(l)}$ <- $w_ij^{(l)} - α \frac{∂E}{∂w_ij^{(l)}}$

- pouziva sa gradientny zostup
  

### Odvodit pravidla pro adaptaci vah v metodě zpětného šíření (alespoň pro výstupní vrstvu + ideu pro skryté vrstvy)

- 

--------------
## 9. RBF sítě
--------------

### Popsat RBF jednotku a architekturu RBF sítě

- RBF jednotka (radial basis function neuron) je specialny typ neuronu, ktory sa aktivuje na zaklade vzdialenosti vstupu od tzv. stredu neuronu. Hodnoti, ako "blizko" je vstup od nejakeho reprezentovaneho bodu, formalne: $y = ρ(∥x−c∥)$, kde $x$ je vstupny vektor, $c$ je stred neuronu, $ρ$ je aktivacna funkcia, typicky Gaussovska: $p(x) = e^{-βx^2}$

- Architektura RBF siete:
  - 1. vrstva: RBF vrstva, ktora pozostava z RBF jednotiek
  - 2. vrstva: vrstva neuronov (perceptronoveho typu)
  - vystup celej siete: $φ(x) = \sum_{i=1}^N w_i ρ(∥x−c_i∥)$, kde $w_i$ su vahy pre vystupnu vrstvu

### Vysvětlit princip trénování RBF sítí

- trening RBF siete prebieha typicky v dvoch fazach:
  1. Vyber stredov (centier) RBF neuronov: pomocou algoritmu k-means sa zhlukuju vstupy do k skupin, stredy tychto zhlukov sa stanu stredmi RBF neuronov $c_i$. Potom spocitame parameter $β$ v kazdom neurone ako $β = \frac{1}{2σ^2}$, kde $σ$ je priemerna vzdialenost objektov v prislusnom zhluku. 

  3. Trening vah vystupnej vrstvy: kedze vystupna vrstva je linearna, mozeme pouzit linearnu regresiu, alebo ak je viac vystupov, maticovu regresiu

### Popsat algoritmus k-means

- Je to algoritmus pre zhlukovanie (ucenie bez ucitela):
  - algoritmus dostane na vstupe vektory dat $x_i$ a cislo $k$, ktore je pocet zhlukov
  - Inicializujeme $k$ nahodnych centier (napr. nahodne vzorky zo vstupu)
  - Opakujeme:
    - Priradenie: kazdy vstupny vektor sa priradi k najblizsiemu stredu
    - Prepocitanie centier: nove centrum kazdeho zhluku je aritmeticky priemer bodov v danom zhluku
  - konvergencia: ked sa centra prestanu menit alebo po danom pocte iteracii 

### Porovnat geometrické vlastnosti RBF sítí a vícevrstvých perceptronů

-

----------------------
## 10. Konvoluční sítě
----------------------

- siete, ktore dostavaju ako vstupy obrazky

### Popsat funkci konvolučního filtru

- Konvolucny filter je mala matica (napr. 3x3), ktora sa "posuva" po obrazku a na kazdom mieste vykonava konvolucnu operaciu - teda vypocet skalarneho sucinu medzi hodnotami pixelov v okienku obrazka a hodnotami filtra (vah)
  - zachytava lokalne obrazove vzory (hrany, texturu, rohy)
  - ma zdielane vahy: roznaky filter sa aplikuje na vsetky pozicie
  - je vypoctovo efektivna
  - vystupom je feature mapa, ktora zvyraznuje vyskyt daneho vzoru v roznych castiach obrazku

### Popsat funkci pooling vrstev v konvoluční síti

- pooling vrstvy redukuju rozlisenie obrazu a tym:
  - znizuju vypoctovu narocnost
  - ulahcuju extrakciu globalnych crt
 
- najcastie pouzivany typ pooling funkcie je max-pooling, ktory pre kazde podokno (napr. 2x2) v obraze vezme maximum z hodnot v tejto oblasti. Typicky sa tieto okna neprekryvaju.

### Popsat architekturu konvoluční neuronové sítě

- architektura pozostava z opakovania blokov:
  - konvolucna vrstva (feature extrakcia)
  - ReLU aktivacia
  - pooling
- na konci funkcia pre klasifikaciu, napr softmax

### Vysvětlit, co jsou matoucí vzory a jak ovlivňují praktické nasazení neuronových sítí

- matuce vzory su obrazky, ktore boli zamerne jemne upravene tak, ze:
  - pre cloveka sa zmeny zdaju neviditelne
  - ale neuronova siet ich klasifikuje nespravne
 
- prakticky to znizuje spolahlivost neuronovych sieti v citlivych oblastiach ako: autonomne vozidla, medicina

### Popsat algoritmus FGSM pro vytváření matoucích vzorů

- FSGM (Fast Gradient Sign Method) je jednoduchy a efektivny sposob, ako vytvorit matuce vzory.

- spocita derivaciu chybovej funkcie podla vstupu neuronovej siete a pripocita k tomuto vstupu $ε sign(∇J)$, kde $∇J$ je gradient, uz len pre $ε < 0.1$ moze lahko zmiast vela modelov neuronovych sieti, ktore davaju dobre vysledky

### Vysvětlit myšlenku přenosu uměleckého stylu

- prenos umeleckeho stylu znamena vytvorenie noveho obrazku, ktory:
  - ide o optimalizacny problem kde sa:
    - zachovava obsah povodneho obrazku
    - ale prevezme styl ineho obrazu (napr. Picasso)

### Vysvětlit základní fungovaní generative adversarial networks (podle rozsahu v textu na webu)

- je architektura dvoch sutaziacich neuronovych sieti:
  - Generator:
    - ma za ulohu generovat obrazky, ktore su podobne obrazkom v nejakej trenovacej mnozine
    - ciel je potom maximalizovat chybu diskriminatoru
  
  - Diskriminator:
    - ma za ulohu poznat, ci predlozeny obrazok je generovany generatorom, alebo ci pochadza z trenovacej mnoziny
    - ciel je minimalizovat svoju chybu

- po trenovani sa vyuziva len generator

--------------------------------
## 11. Rekurentní neuronové sítě
--------------------------------

### Definovat, co jsou rekurentní neuronové sítě

- su take neuronove siete, v ktorych existuju cyklicke vazby medzi neuronmi, teda vahy vedu spat do predchadzajucich stavov. Vdaka tomu si siet pamata predchadzajuce vstupy, teda ma vlastny vnutorny stav

### Vysvětlit, kde se rekurentní sítě používají

- Pouzivaju sa vsade tam, kde je vstup sekvencny alebo casovy, resp. kde je vhodnejsie sieti pridat nejaky vnutorny stav, reprezentovany pomocou spoja, ktory vedie spat:
  - predikcia casovych radov
  - strojovy preklad
  - rozpoznavanie reci
  - generovanie textu

### Vysvětlit trénování rekurentních sítí pomoci algoritmu zpětného šíření v čase

- Klasicky backpropagation algoritmus priamo nefunguje, lebo vahy sa opakovane pouzivaju v case. Riesenie je rozvinut siet v case - vytvorit si kopiu siete pre kazdy casovy krok.

### Vysvětlit problém s explodujícími a mizejícími gradienty

- Pri backpropagation through time (BPTT) sa gradienty opakovane nasobia tymi istymi vahami v kazdom kroku (ak ide o rekurentnu vahu), to sposobuje:
  - explodujuce gradienty: ak vahy > 1 -> gradienty rastu exponencialne
  - miznuce gradienty: ak vahy < 1 -> gradienty klesnu k nule
 
- riesenie: bud pouzijeme Echo State siete alebo sa rekurentna vaha zafixuje na 1 a praca so stavom siete sa prevadza explicitne ako v Long-short term memory sietach

### Popsat princip Echo State sítí

- specialny typ rekurzivnych neuronovych sieti

- hned na vstupe maju velku rekurentnu vrstvu s nahodnymi fixnymi vahami, ktore sa dalej netrenuju

- typicky je tato vrstva implementovana ako jedina matica $k x m$, kde $k = n+m$, $m$ je velkost rekurentnej vrstvy a $n$ je pocet vstupov

- tradicne ESN po rekurentnej vrstve obsahuje uz len jednu vrstvu, ktoru podobne ako u RBF sieti mozeme trenovat pomocou linearnej regresie

### Popsat trénování Echo State sítí

- siet dostane vektory dlzky $n$ a pripoji k nim svoj vnutorny stav dlzky $m$, na ten aplikuju svoju rekurentnu vrstvu a tym dostane novy vnutorny stav

### Popsat LSTM buňku a architekturu LSTM sítě

- nahradia kazdy neuron tzv. LSTM bunkou, ktora explicitne pracuje so svojim stavom a rekurentne spoje medzi jednotlivymi bunkami potom maju vahu zafixovanu na 1

- vstupom kazdej bunky je jej stav $c_{t-1}$ z predosleho kroku a spojenie ich vystupov v predchadzajucom stave a novych vstupov $[h_{t-1}, x_t]$

- na zaklade vstupu sa najprv spocitaju hodnoty tzv. bran (gates) - zapominacia $f_t$ (forget - rozhodne, co z predosleho stavu sa zabudne) a vstupni $i_t$ (input - co nove sa zapise do stavu)

- // TODO: vzorce

### Vysvětlit výhody Echo State sítí a LSMT sítí v porovnání se základními rekurentními sítěmi

- no asi najvacsia vyhoda je prave ta, ze sa dokazeme vyhnut explodujucim a miznucim gradientom

-------------------
## 12. Neuroevoluce
-------------------

### Popsat použití evolučních algoritmů pro trénování vah v neuronové síti

- Majme nejaku doprednu neuronovu siet

- Ak chceme pouzit evolucne algoritmy, vpodstate pojde o problem spojitej optimalizacie, kde musime dodrzat postup:
  - Populacia: Kazdy jedinec je nejaky vektor realnych cisel
  - Geneticke operatory:
    - Mutacia: ku kazdemu genu sa pripocita nahodna hodnoa z normalneho rozdelenie
    - Krizenie: sa nepouziva, pretoze ich adaptacia moze byt dost velka
  - Fitness funkcia: jedine sa ohodnoti podla vykonu siete (napr. skore v hre)
 
- vyhody oproti gradiennym metodam:
  - lepsia paralelizacia
  - hodi sa pre prostredie s riedkymi odmenami
  - nevyzadujeme vypocet derivacii
   
### Popsat algoritmus NEAT - reprezentace jedince, operátory

- evolvuje vahy aj topologiu siete naraz
- reprezentacia jedinca:
  - je reprezentovany zoznamom, ktory v kazdom gene obsahuje informacie:
    - odkial ide
    - kam vedie
    - hodnota, resp. vaha
    - priznak, ci je spoj aktivny
    - inovacne cislo

- inicializacia siete:
  - NEAT zacina z minimalnych struktur (neuronovych siti), ktore neobsahuju ziadne skryte neurony a potom sa rozsiruje
      
- geneticke operatory:
  - mutacia:
    - pridanie hrany: nahodne spoji dva neurony, ak spoj este neexistuje
    - pridanie neuronu: rozdeli existujuci spoj, deaktivuje ho, prida novy neuron a dve nove hrany
 
  - krizenie: robi sa pomocou invoacnych cisel:
    - dvaja jedinci, ktori sa maju krizit sa zarovnaju podla inovacnych cisel
    - hrany s rovnakym inovacnym cislom, ktore su u oboch jedincov sa dedia nahodne z jedneho alebo druheho
    - hrany, ktore su iba v jednom z jedincov sa dedia z toho lepsieho z tych dovch
    - to, ci su hrany aktivne alebo neaktivne sa rozhodne podla zdedenej hrany s nejakou pravdepodobnostou

- rozdelenie populacie na viacej druhov:
  - jedinci kazdeho druhu explciitne zdielaju fitness, teda fitness kazdeho jedinca je vydelena poctom jedincov rovnakeho druhu a tym sa dava novo objavenym myslienkam (strukturam) cas pre to, aby sa vylepsily pomocou genetickych operatorov 

### Vysvětlit význam inovačních čísel v algoritmu NEAT

- inovacne cislo je identifikator kazdej hrany, urceny podla poradia, v ktorom vznikol pocas mutacii. Sluzi na:
  - zarovannie rodicov pri krizeni
  - rozlisenie medzi starymi a novymi mutaciami

### Popsat myšlenku algoritmu HyperNEAT

- HyperNEAT je rozsirenie NEAT, ktore oddeluje vyvoj topologie siete od vypoctu konkretnych vah

- myslienka:
  - neuronova siet, kroru chceme vytvorit ma fixnu topologiu na zaciatku (napr. mriezku/hyper-kocka)
  
  - vahy sa reprezentuju pomocou inej neuronovej siete
    - tato siet dostava na vstup suradnice neuronov vo vyslednej sieti a vracia pre ne priamo vahy
    - siet pre vypocet vah sa potom vytvara pomocou NEATu

### Popsat rozšíření algoritmu NEAT pro hledání architektur neuronových sítí (algoritmus coDeepNEAT)

- hladame celu strukturu neuronovych sieti

- z hladiska evolucnych algoritmov je potom potreba zakodovat strukturu siete (na urovni vrstiev alebo blokov) do jedinca

- v pripade vrstiev by stacilo, ak by bol jedinec napr. postupnost tychto vrsiev vratane ich parameterov

- operatory su potom upravy parametrov tychto vrstiev, pridanie dalsiej vrstvy, alebo pridanie spojenia medzi vrstvami, ktore spolu nesusedia

- algoritmus CoDeepNEAT koduje jedinca podobne ako NEAT. Rozdiel je v tom, ze jednotlive uzly neobsahuju jednotlive neurony, ale priamo moduly neuronovej siete (bloky neuronov)

- tieto moduly su taktiez vytvarane pomocou NEAT algoritmu

- pri vyhodnocovani fitness sa celkovy jedinec vytvori pomocou kombinacie tychto modulov, tak, ako je popisane v jedincovi.

### Vysvětlit myšlenku algoritmu novelty search

- princip je vynechanie fitness funkcie, ktora by priamo hodnotila kvalitu najdeneho riesenia

- tato fitness funkcia je nahradena funkciou, ktora porovnava chovanie vytvorenych jedincov (napr. v bludisku vzdialenost start/end pozicii)

### Porovnat novelty search a evoluční algoritmy a diskutovat jejich výhody/nevýhody

- Vyhody novelty searchu: vyhyba sa lokalnym optimam fitness funkcie, podporuje diverzitu

- Vyhody evolucnych algoritmov: ma silne spojenie s cielovou ulohou

----------------------------------
## 13. Hluboké zpětnovazební učení
----------------------------------

### Vysvětlit algoritmus hlubokého Q-učení

- rovnaka myslienka ako u Q-ucenia, tj. chceme pre kazdy stav $s$ a kazdu akciu $a$ odhadnut aku diskontovanu odmenu by sme dostali, ak by sme v stave $s$ vykonali akciu $a$. V klasickom Q-uceni tieto hodnoty boli reprezentovane ako matice Q, tato reprezentacia je problematicka, ak je vela stavov alebo vela akcii. V hlbokom Q-uceni sa teda pre reprezentaciu tejto matice pouziva neuronova siet, ktora pre kazdy stav vrati ohodnotenie vsektych akcii.

- ciel je naucit agenta policy, ktora maximalizuje ocakavanu diskontovanu odmenu v prostredi MDP (Markov decision problem) pomocou aproximacie Q-funkcie neuronovou sietou

### Porovnat Q-učení a hluboké Q-učení

- Q-ucenie aproximuje hodnotovu funkciu pomocou tabulky, zatial co hlbkoe Q-ucenie pouziva neuronovu siet

- Q-ucenie funguje fajn pre male a diskretne priestory, hlboke funguje aj pre vacsie

### Popsat triky s experience replay a target network

- target network:
  - podstatou je, ze oddelime siet pre vyber akcie a siet pre odhad hodnoty
  - typicky len tak, ze sa zafixuju parametre siete, podla ktorej sa vybera akcia a menime len tie parametre siete, ktora sa uci ohodnocovanie

- experience replay:
  - podstatou je, ze si pamatame stvorice $(s,a,r,s')$ stavu,akcie,odmeny a nasledujuceho stavu a pri trenovani nahodne vyberiem prechody z tejto pamati miesto toho, aby sme vzdy pouzivali posledny prechod

### Vysvětlit vliv experience replay a target network na hluboké Q-učení

- zachovanie vacsej stability pri trenovani

### Popsat algoritmus DDPG

- myslienka je taka, ze ak mame priestor akcii spojity (napr. riadenie auta, kde akcia je uhol otocenia volantu a stlacenie plynu/brzdy), tak mame velmi vela akcii, co je velmi zlozite na vyratanie maxima. Preto existuje algoritmus DDPG, ktory nahradi tuto operaciu novou sietou, ktora priamo vrati akciu, ktora sa ma previest

- myslienka trenovania je taka, ze chceme aby algoritmus vratil akciu, ktora maximalizuje $Q_θ (s,a)$

- pouziva experience replay a target network

### Popsat actor-critic přístupy

- idea kombinuju vyhody policy gradientu a hodnotovych metod

- $G_t$ moze mat velky rozptyl -> nestabilne ucenie

- preto nahradime $G_t$ inymi vyrazmi, jedna moznost je priamo hodnota $Q(s,a)$, druha je napr. advantage

### Popsat advantage a zdůvodnit, proč se používá

- spocita sa ako rozdiel medzi prevedenim akcie $a_t$ v stave $s_t$ a hodnotou stavu: $V(s_t) - A(s_t, a_t) = Q(s_t, a_t) - V(s_t)

- pocitame teda o kolko je lepsie vykonat akciu $a_t$ oproti nejake obecnej akcii. 

### Popsat metodu A3C

- nahradzuje pouzitie experience replay tym, ze sa hraje viacero hier naraz, aktualizacia vah sa potom priemeruje cez aktualizacia spocitane v vsetkych nezavislych hrach

----------------------------------
## 14. Particle Swarm Optimization
----------------------------------

- algoritmus pre spojitu optimalizaciu

- inspirovany chovanim ryb/vtakov

- populacia je reprezentovana ako mnozina castic

### Popsat aktualizaci poloh a rychlostí v PSO

- Kazda castica (particle) ma svoju poziciu $x$, rychlost $v$, najlepsiu najdenu poziciu $p_{best}$ a najlepsiu poziciu celej populacie $g_{best}$

- Algoritmus potom prebieha tak, ze sa castice pohybuju v priestore a pritom su pritahovane k miestam, kde nasla svoje najlepsie riesenie a globalne najlepsie riesenia

- Inicializujeme nahodnu pociatocnu populaciu castic a iterative aktualizujeme rychlosti a pozicie vsetkych castic podla rovnic:
  - $v$ <- $ωv + φ_p r_p(p_{best} - x) + φ_b r_b(g_{best} - x)$
  - $x$ <- $x + v$
 
- kde $r_p$, $r_b$ su nahodne cisla medzi 0 a 1
- $ω$, $φ_p$, $φ_b$ su parametre, ktore urcuju zotrvacnost a ako velmi je jedinec pritahovany k svojmu a ku globalnemu najlepsiemu rieseniu

### Popsat topologie používané v PSO (globální, geometrické, sociální)

- topologia urcuje, ktore casti si medzi sebou dokazu komunikovat a zdielat o sebe informacie

- globalna topologia:
  - Vsetky castice mozu priamo komunikovat so vsetkymi (a sdielat informaciu o globalnom optime)
  
- geometricka topologia:
  - Spolu komunikuju castice, tkore su blizko u seba

- socialna topologia:
  - Topologia je urcena predom, bez ohladu na pozicie castic, napr. kruhova topologia, kde kazda castica ma iba dvoch susedov

### Vysvětlit vliv topologií na algoritmus PSO

- Globalna zaistuje rychlu konvergenciu, ale castice casto sa dostanu len do lokalneho minima

- Geometricke a socialne, viac hejn, vacsia diverzita, nevyhoda ale je ze im to trva trocha dlhsie, teda pomalsia konvergencia

### Vysvětlit použití PSO pro řešení problémů ve spojité optimalizaci

- Pozicie castic priamo reprezentuju kandidatov na riesenie

- Kvalita kandidatov je urcena podla hodnoty optimalizovanej funkcie - fitness funkcia sa rata pre kazdu casticu na zaklade jej pozicie

------------------------------
## 15. Ant Colony Optimization
------------------------------

### Vysvětlit základní pojmy ACO - feromon, atraktivita

- zalozeny na chovani mravcov

- Feromon $𝜏_{xy}$: mnozstvo latky na hrane (x,y), ktore urcuje, ako casto bola hrana vyuzivana mravcami, ktori nasli dobre riesenie. Pohyb funguje tak, ze mravec poklada male mnozstvo feromonu, ak najde poravu, tak sa vracia do mraveniska a ide znova za potravou, ale poklada ovela vacsie mnozstvo feromonu.

- Atraktivita $ν_{xy}$: heuristicka hodnota prechodov medzi vrcholmi, cim vyssia tym lepsia

- algoritmus bezi iterativne - vygenerovanie riesenia a update feromonu

### Popsat generování řešení pomocí ACO

- Kazdy mravec zacne v nahodnom uzle grafu

- Postupne si vybera dalsi uzol na zaklade pravpodobnosti prechodu z vrcholu $x$ do vrcholu $y$, ktora zavisi na mnozstve feromonu $𝜏_{xy}$ medzi tymito dvoma vrcholmi a atraktivite 

- Pravdepodobnnost prechode je umerna hodnote $(𝜏_{xy}^α)(ν_{xy}^β)$, kde $α$ je konstanta reprezentujuca vahu feromonu a $β$ je konstanta reprezentujuca vahu atraktivity 

### Popsat aktualizaci feromonu na základě kvality nalezeného řešení

- Update feromonu ma dva kroky - vyparenie feromonu a polozenie noveho feromonu

- Vyparenie feromonu:
  - $𝜏_{xy}$ <- $(1-ρ)𝜏_{xy}$, kde ρ je konstanta, ktora urcuje rychlost vyparenia feromonu z intervalu [0,1] 

- Polozenie noveho feromonu, ktore odpoveda kvalite riesenia najdenych mravcov:
  - $𝜏_{xy}$ <- $\sum_k 𝜏_{xy}^k$, kde $Q$ je vhodne zvolena konstanta a $L_k$ je kvalita riesenie najdeneho mravcom $k$ a $𝜏_{xy}^k = \frac{Q}{L_k}

### Popsat použití ACO pro řešení problému obchodního cestujícího a vehicle routing problému

- TSP:
  - Uzly = mesta
  - Hrany = cesty medzi mestami
  - $L_k$ je typicky dlzka cesty
  - atraktivita $ν_{xy}$ je prevratena hodnota dlzky hrany (x,y)

- VHP:
  - riesi sa velmi podobne ako TSP, ale s viacerymi vozidlami a obmedzeniami (napr. kapacita vozidla)
  - Feromony a heuristika sa vyuzivaju rovnako
  - Jediny rozdiel je v generovani cesty mravcom, mame tam obmedzenie na kapacitu, tak len pridame, ze ak presiahneme kapacitu, tak sa vratime do skladu

-----------------------
### 16. Artificial Life
-----------------------

- zabyva sa systemami, ktore pripominaju skutocny zivot
- cielom je pochopit ako zivot funguje a komunikuje, pripadne zivot vytvorit

### Vysvětlit rozdíly mezi jednotlivými typy umělého života (soft, hard, wet)

- Soft Alife (softwarovy):
  - vyuziva pocitacove simulacie, v ktorych sa modeluje chovanie organizmov a ich interakcie (napr. celularne automaty, Tierra, Game of Life
  - cielom je pochopit principy zivota a evolucie cez simulacie

- Hard Alife (hardwarovy):
  - zameriava sa na konstrukciu fyzickych robotv alebo zariadeni, ktore napodobnuju zivotne chovanie (napr. roboty)
  - cielom je vytvorit embodiment zivota

- Wet Alife (mokry):
  - pracuje s biochemickymi latkami a experimentuje s tvorbou zivota na chemickej urovni (napr. pokusy vytvorit umelu DNA)
  - inspiruje sa prirodnou biochemiou a usiluje sa o nove formy zivota na molekularnej urovni

### Vysvětlit rozdíly mezi silným a slabým umělým životem

- weak Alife:
  - Hovori, ze zivot nie je mozne vytvorit mimo biologicky kontext
  - Simulacie su iba nastroje ku studiu zivota, ale nie su zive sami o sebe

- strong Alife:
  - Hovori, ze zivot je formalny proces, ktory moze existovat v lubovolnom mediu
  - napr. softwarovy organizmus moze byt povazovany za zivy, ak vykazuje potrebne vlastnosti zivota (reprodukcia, vyvoj,...)

### Popsat 1D a 2D celulární automaty

- Celularni automat je diskretny model, tvoreny mriezkou buniek, kazda bunka ma stav (napr. farba) a ten sa aktualizuje podla pravidiel podla stavov ich susedov

- 1D celularny automat:
  - Mriezka je jednorozmerna

- 2D celularny automat:
  - Bunky tvoria 2D mriezku

### Popsat pravidla Game of Life

- 2D celularni automat s dvoma stavmi (ziva/mrtva bunka)
- Pravidla:
  - Ziva bunka s 2 alebo 3 zivymi susedmi preziva
  - Ziva bunka s menej ako 2 alebo viac ako 3 zivymi susedmi umiera
  - Mrtva bunka s prave 3 zivymi susedmi ozije

### Popsat pravidla Langtonova mravence

- Je to mravec, ktory sa pohybuje na 2D mriezke s dvoma farbami

- Pri kazdom pohybe mravec prefarbi svoje policko na opacnu farbu a v zavisosti na povodnej farbe policka sa rozhodne ci sa ma otocit doprava/dolava

### Vysvětlit pojem dálnice u Langtonova mravence

- Mravec na zaciatku sa hybe podla pravidiel a jednoducho, potom sa ale zacne chovat chaoticky a pseudonahodne.
  
- Nakoniec ale zacne opakovat postupnost 104 krokov, ktore tvoria tzv. dialnicu: mravec tym uteka stale jednym smerom na nekonecna

### Popsat systém Tierra

- system Tierra simuluje zivot pocitacovych programov
- je implementovana ako emulator jednoducheho pocitacoveho kodu s 32 roznymi instrukciami
- tieto instrukcie obsahuju jednoduche aritmeticke instrukcie, podmienky, skoly a dva typy no-op - NOP0 a NOP1 - tie sa pouzivaju na definovanie cielov skokov
- simulacia zacina s jednym jedincom, ktory sa dokaze sam skopirovat

### Popsat jedince v systému Tierra

- jedinec je program zlozeny z instrukcii
- ma svoj vlastny virtualny procesor

### Vysvětlit způsob adresování (definování cílů skoku) v systému Tierra

- nepouziva absolutne alebo relativne adresy pre skoky
- miesto toho poziva instrukcie NOP0 a NOP1 ako definovanie cielov skokov
- ak mame niekde instrukciu pre skok, nasleduje po nej niekolko NOPx instrukcii, pri skoku potom hladame najblizsie miesto v pamati, kde je komplementarna postupnost instrukcii (NOP0 je komplementrana k NOP1 a naopak)

### Popsat příklady vytvořených jedinců v sytému Tierra (paraziti)

- simulacia zacina s jednym jedincom, ktory sa dokaze skopirovat

- pri kopirovani je mala sanca, ze dojde k mutacii bitu v jedinci, tym ale mozu vzniknut novi, niektori su zaujimavi:
  - paraziti: neobsahuju vlastnu kopirovaciu proceduru, ale ak populacia obsahuje jedincov s touto procedurou, tak paraziti preziju
 
  - jedinci, ktori sa dokazu branit parazitom
 
  - hyper-paraziti: presvedcia parazita, abz kopiroval ich miesto seba
