# 12. Costruttori logici (restrizioni di proprietà): [some](#costsomes), [only](#costonly), [exactly](#costexctl), [min](#minmax), [max](#minmax)

### Ultimo aggiornamento del 20 Maggio 2026 alle ore 16:45

---
<p>

I costruttori logici sono fondamentali per spiegare al reasoner i vincoli del mondo reale.<br>
La sintassi di base è <br>
<b>Proprietà</b> - <b>Parola Chiave</b> - <b>Tipo di dato</b>.<br>
Per esempio, <br>
hasMatricola <b>exactly 1</b> xsd:Integer <br>
significa che un individuo deve avere una e una sola matricola composta esclusivamente da numeri interi (1234567890).
</p>

<span id="costsome"><h3>Il costruttore <i>some</i> (restrizione esistenziale)</h3></span>
<p>
 Si definisca studente un individuo che deve seguire almeno un corso.<br>

La parola chiave <code>some</code> corrisponde al simbolo $\exists$ e significa <b>"almeno uno"</b>.<br>
Nell'esempio che segue, serve per dire al reasoner che uno studente segue <b>ALMENO</b> un corso.<br>

Per inserire una restrizione esistenziale <code>some</code>:
<ol>
<li>Clicchiamo su <b>Entities</b> > <b>Classes</b> > clicchiamo sulla classe <b>Studente</b>;</li>
<li>nel riquadro <b>Description</b> clicchiamo sulla + a destra di <b>SubClass Of</b>;</li>
<li>digitiamo <code>segueCorso some Corso</code>, infine confermiamo.</li>
</ol>

![](../pics/12_1.png)<br>
Apparirà quindi una nuova riga nella sezione <b>SubClass Of</b> di <b>Studente</b>

Poiché il ragionatore vede che <code>CiccioPasticcio</code> è uno <code>Studente</code>, non riceveremo alcun errore: HermiT ipotizza che <code>CiccioPasticcio</code>, data la sua appartenenza alla classe <code>Studente</code>, segua <b>ALMENO</b> un corso.<br>
![](../pics/12_2.png)
</p>

<span id="costonly"><h3>Il costruttore <i>only</i> (restrizione universale)</h3></span>
<p>
La restrizione universale <b>only</b> significa "solo ed esclusivamente" e si indica con ∀<br>

Per fare un esempio, introduciamo due nuove classi nell'ontologia:
<ol>
<li>una classe <code>CorsoTelematico</code> sorella di <code>Corso</code> e disgiunta da essa;

![](../pics/12_3.png)
</li>
<li><code>StudenteTelematico</code>, figlia di <code>Persona</code>, con SubClass Of <code>Persona</code> e <code>segueCorso only CorsoTelematico</code> di cui <code>CiccioPasticcio</code> è un'istanza (attenzione, <code>CiccioPasticcio</code> è SOLO uno <code>StudenteTelematico</code> e non uno <code>Studente</code>).

![](../pics/12_4.png)
</li>
</ol>

All'avvio del reasoner, vedremo che <b>StudenteTelematico</b> può seguire solo un <b>CorsoTelematico</b>, anche perché nel <a href="./04_creazione_proprieta.md">capitolo 4</a> abbiamo definito <code>Corso</code> come <b>range</b> di <code>segueCorso</code>.<br>
Inoltre, Analisi I appartiene alla classe <code>Corso</code>.<br>
![](../pics/12_5.png)
</p>

<span id="costexctl"><h3>Il costruttore <i>exactly</i> (cardinalità esatta)</h3></span>
<p>
<b>exactly</b> ci permette di fissare un numero ben preciso di relazioni che un individuo può avere con una determinata proprietà.<br>

Aggiungere una restrizione di tale tipo non è difficile: basta specificarla come sottoclasse di una <b>classe</b>.<br>
In questo esempio, abbiamo specificato che la <b>classe</b> <code>Studente</code> può contenere esattamente un solo dato <code>haMatricola</code> di tipo xsd:integer.<br>
![](../pics/12_6.png)

Se proveremo ad assegnare più di una matricola a <code>CiccioPasticcio</code>, il reasoner andrà semplicemente in errore come da schermata.
![](../pics/12_7.png)
</p>

<span id="minmax"><h3>Il costruttore <i>min</i> e <i>max</i> (cardinalità minima e massima)</h3></span>
<p>
<b>min</b> = minimo<br>
<b>max</b> = massimo<br>
Facciamo qualche esempio pratico e dimostriamo la fallibilità di HermiT dinanzi al costrutto max.<br>
Dovremo performare alcune azioni nella nostra ontologia al fine di creare le circostanze adatte:
<ol>
<li>andiamo su <b>Entities</b> > <b>Classes</b> > creiamo una <b>classe</b> <code>Tesi</code>;</li>
<li>creiamo tre <b>individui</b> <code>TesiA, TesiB, TesiC</code> appartenenti alla <b>classe</b> <code>Tesi</code> e chiariamo che sono individui diversi tra loro col menù <b>Different Individuals</b> (basterà farlo una sola volta, si consiglia di selezionare le altre due tesi con il tasto CTRL o MAIUSC);

![](../pics/12_8.png)
</li>
<li>andiamo su <b>Entities</b> > <b>Classes</b> > creiamo una <b>classe</b> <code>Professore</code> figlia della <b>classe</b> <code>Persona</code>;</li>
<li>andiamo su <b>Entities</b> > <b>Object properties</b> > creiamo una <b>proprietà oggetto</b> <code>relatoreDi</code> > impostiamo <code>Professore</code> come <b>Dominio</b> > impostiamo <code>Tesi</code> come <b>Range</b>;

![](../pics/12_9.png)
</li>
<span id="punto5pp"><li>creiamo un <b>individuo</b> <code>ProfPico</code> appartenente alla <b>classe</b> <code>Professore</code>, poi aggiungiamo le tre <b>asserzioni</b> visibili sulla parte destra della schermata;

![](../pics/12_10.png)
</li></span>
<li>torniamo alla <b>classe</b> <code>Professore</code> e definiamo il limite <code>relatoreDi max 2 Tesi</code>

![](../pics/12_11.png)
</li>
</ol>
Adesso, avviamo il reasoner.

![](../pics/12_12.png)<br>
Il costrutto <b>max</b> sta funzionando alla perfezione: il professore Pico può essere relatore di <b>massimo</b> due tesi.<br>
Vi basterà togliere una tesi dalle asserzioni dell'individuo <code>ProfPico</code> [(schermata al punto 5)](#punto5pp) per far tornare il reasoner alla normalità.<br>

Per quel che riguarda <b>min</b>, invece, il reasoner NON fallirà assolutamente nel caso in cui, per esempio, dovessimo impostare <code>relatoreDi min 2 Tesi</code> e l'individuo <code>ProfPico</code> come relatore di una sola tesi: HermiT inferirà a priori che <code>ProfPico</code> sia relatore di minimo due tesi. 
</p>
________________
<h3><a href="https://www.youtube.com/watch?v=dQw4w9WgXcQ">Passa al capitolo successivo</a></h3>
<h3><a href="./11_disgiunzione.md">Ritorna al capitolo precedente</a></h3>
<h3><a href="../README.md">Ritorna all'indice</a></h3>