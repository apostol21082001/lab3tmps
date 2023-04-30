## Laborator TMPS  №3

*Pregatit de Apostol Mihail, grupa TI-204*

*Scopul acestui laborator a fost implementarea a 4 modele structurale (Structural Patterns)*

Au fost  folosite 5 șabloane in acest proiect:
4 Structutural Patterns si unul Behavioral Pattern(Memento):

## 1. Decorator
![Decorator](https://refactoring.guru/images/patterns/cards/decorator-mini-2x.png)

Un decorator este un model de design structural care vă permite să adăugați în mod dinamic noua funcționalitate obiectelor, împachetându-le în ambalaje utile.
Decoratorul are un nume alternativ - un ambalaj. Descrie mai precis esența modelului: puneți obiectul țintă într-un alt obiect de înveliș care rulează comportamentul de bază al obiectului și apoi adăugați ceva propriu la rezultat.

Ambele obiecte au o interfață comună, așa că nu are nicio diferență pentru utilizator dacă obiectul este pur sau ambalat. Puteți utiliza mai multe ambalaje diferite în același timp - rezultatul va avea comportamentul combinat al tuturor ambalajelor simultan.

Cu ajutorul unui decorator, înfășurăm o funcție într-o alta cu funcționalitate suplimentară

Funcția Draw include mai multe apeluri interne de metodă.
```
Draw(){
        this.Context.beginPath();
        this.Context.arc(this.X,this.Y,this.D,0,2*Math.PI);
        this.Context.stroke();
        this.Context.fillStyle = new ColorAdapter(this.Color).Hex;
        this.Context.fill();
    }
```
## 2. Bridge
![Bridge](https://refactoring.guru/images/patterns/cards/bridge-mini-2x.png)

Bridge este un model de design structural care separă una sau mai multe clase în două ierarhii separate - abstractizare și implementare, permițându-le să fie schimbate independent una de cealaltă.

Un exemplu de problemă în care acest model poate fi utilizat:

Aveți o clasă Forme geometrice care are subclase Cerc și Pătrat. Doriți să extindeți ierarhia formelor după culoare, adică să aveți forme roșu și albastru. Dar pentru a pune totul împreună, va trebui să creați 4 combinații de subclase, cum ar fi Cercuri albastre și pătrate roșii.

![Bridge](https://refactoring.guru/images/patterns/diagrams/bridge/problem-en-2x.png)

Odată cu adăugarea de noi tipuri de forme și culori, numărul de combinații va crește exponențial. De exemplu, pentru a introduce forme de triunghi în program, va trebui să creați două noi subclase de triunghiuri simultan pentru fiecare culoare. După aceea, noua culoare va necesita crearea a trei clase pentru toate tipurile de forme. Mai departe mergem, devine mai rău.

Bridge ne-a permis să implementăm derivarea parametrilor într-o clasă separată.

Clasa Circle include parametrii ColorAdapter și Color, care sunt clase care identifică o culoare.

```
class Circle {
    constructor(X, Y, D, Color, Context) {
        this.History = [this];
        this.X = X;
        this.Y = Y;
        this.D = D;
        this.Color = Color;
        this.Adapter = new ColorAdapter(this.Color);
        this.Context = Context;
    }
}
```

## 3. Iterator
![Iterator](https://refactoring.guru/images/patterns/cards/iterator-mini-2x.png)

Un iterator este un model de design comportamental care face posibilă traversarea secvențială a elementelor obiectelor compuse fără a expune reprezentarea lor internă.

Ideea din spatele modelului Iterator este de a muta comportamentul de traversare a colecției din colecția în sine într-o clasă separată.

![Iterator](https://refactoring.guru/images/patterns/diagrams/iterator/solution1-2x.png)

Iteratorul ne oferă posibilitatea de a implementa implementarea diferitelor treceri prin foaie

Acest model este implementat de următoarele interfețe:

```
interface IIterator{
    next();
    hasMore();
}

export default IIterator
```

Și interfața este implementată de clasă

```
import IITerator from './IITerator'

class Iterator implements IITerator{
    private currentPos = 0;
    private Collection:Array<any> = new Array();

    Iterator(){ }

    next():void{
        if(this.currentPos < this.Collection.length){
            return this.Collection[this.currentPos++]
        }
        else{
            this.currentPos = 0
        }
    }
    push(item:any):void{
        this.Collection.push(item)
    }
    pop():void{
        this.Collection.pop()
    }
    hasMore():Boolean{
        if(this.currentPos < this.Collection.length){
            return true;
        }
        else{
            return false;
        }
    }
}

export default Iterator
```

## 4. Memento
![Iterator](https://refactoring.guru/images/patterns/cards/memento-mini-2x.png)
Pattern din Behavioral Patterns

Memento — este un model de design comportamental care vă permite să salvați și să restaurați stările trecute ale obiectelor fără a dezvălui detaliile implementării lor.
Un exemplu de problemă în care acest model poate fi utilizat:

Să presupunem că scrieți un program de editare de text. Pe lângă editarea obișnuită, editorul vă permite să schimbați formatarea textului, să inserați imagini și multe altele.

La un moment dat ai decis să anulezi toate aceste acțiuni. Pentru a face acest lucru, trebuie să salvați starea curentă a editorului înainte de a efectua orice acțiune. Dacă utilizatorul decide apoi să-și anuleze acțiunea, veți prelua o copie a stării din istoric și veți restabili starea veche a editorului.

![Iterator](https://refactoring.guru/images/patterns/diagrams/memento/problem1-ru-2x.png)

Memento ne permite să salvăm istoricul obiectului, starea lui anterioară

Acest model este implementat de interfață

```
interface IMementoCircle{
    getName():string;
    getSnapshot(version:number)
}

export default IMementoCircle;
```

Și interfața este implementată
```
class Circle implements IMementoCircle{
    ...
    getName(){
        return "Circle";
    }

    getSnapshot(version:number){
        if(this.History.length > version){
            return this.History[version];
        }
        else{
            return this;
        }
    }
}
```

## 5. Adapter
![Adapter](https://refactoring.guru/images/patterns/cards/adapter-mini-2x.png)

Un adaptor este un model de design structural care permite obiectelor cu interfețe incompatibile să lucreze împreună.

Un exemplu de problemă în care acest model ar putea fi util:

  Imaginați-vă că faceți o cerere de tranzacționare la bursă. Aplicația dvs. descarcă cotații bursiere din mai multe surse în XML și apoi desenează grafice frumoase.

La un moment dat, decideți să îmbunătățiți aplicația utilizând o bibliotecă de analiză terță parte. Dar aici este problema - biblioteca acceptă doar formatul de date JSON, care este incompatibil cu aplicația dvs.

![Adapter](https://refactoring.guru/images/patterns/diagrams/adapter/problem-2x.png)

Puteți rescrie biblioteca pentru a accepta formatul XML. Dar mai întâi, poate rupe codul existent care depinde deja de bibliotecă. Și în al doilea rând, este posibil să nu aveți acces la codul sursă.

Aveam nevoie de un adaptor pentru a putea arunca cu forță un obiect pe interfața altuia

### Hex color Interface
```
interface IColorHEX{
    Hex:string;
}

export default IColorHEX;
```

### RGB color Interface
```
interface IColorRGB{
    Red:number;
    Green:number;
    Blue:number;
}

export default IColorRGB;
```

### Clasa de adaptor de culoare care permite combinarea RGB și HEX

```
class ColorAdapter implements IColorHEX,IColorRGB{
    public Red:number;
    public Green:number;
    public Blue:number;
    public Hex:string;
}
```
