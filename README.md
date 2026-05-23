# Java_utazok
```
import java.io.File;
import java.io.FileNotFoundException;
import java.io.PrintWriter;
import java.util.ArrayList;
import java.util.Scanner;
import java.util.TreeMap;

public class Main {
    public class Utazas {
        public String nev;
        public String varos;
        public String datum;
        public String idopont;

        public Utazas(String sor) {
            String[] s = sor.split(";");
            nev = s[0];
            varos = s[1];
            datum = s[2];
            idopont = s[3];
        }
    }

    private ArrayList<Utazas> utazasok = new ArrayList<>();

    public Main() {
        // --- 0. feladat ---
        betolt("utazok.csv");
        System.out.printf("0) %d utazó adata beolvasva\n", utazasok.size());

        // --- 1. feladat ---
        ArrayList<String> varosok = new ArrayList<>();
        for (Utazas u : utazasok) {
            if (!varosok.contains(u.varos)) {
                varosok.add(u.varos);
            }
        }
        System.out.printf("1) Összesen %d városba utaztak\n", varosok.size());

        double veletlen = Math.floor(Math.random() * varosok.size());
        System.out.printf("   Közülük egy véletlenszerűen kiválasztott: %s\n", varosok.get((int) veletlen));

        int veletlenVarosbaErkezok = 0;
        for (Utazas u : utazasok) {
            if (u.varos.equals(varosok.get((int) veletlen))) {
                veletlenVarosbaErkezok++;
            }
        }
        System.out.printf("   Ebbe a városba %d utazó érkezett\n", veletlenVarosbaErkezok);

        // --- 2. feladat ---
        Utazas legkorabbiIndulas = utazasok.getFirst();
        for (Utazas u : utazasok) {
            if (u.idopont.compareTo(legkorabbiIndulas.idopont) < 0) {
                legkorabbiIndulas = u;
            }
        }
        System.out.printf("2) Legkorábbi indulás: %s\n", legkorabbiIndulas.idopont);

        int delelottiIdopontok = 0;
        for (Utazas u : utazasok) {
            if (u.idopont.compareTo("12:00") < 0) {
                delelottiIdopontok++;
            }
        }
        System.out.printf("   Összesen %d utazás kezdődött délelőtt\n", delelottiIdopontok);

        // --- 3. feladat ---
        TreeMap<String, Integer> statisztika = new TreeMap<>();
        for (Utazas u : utazasok) {
            String honap = u.datum.split("\\.")[0];
            if (!statisztika.containsKey(honap)) {
                statisztika.put(honap, 1);
            } else {
                statisztika.put(honap, statisztika.get(honap)+1);
            }
        }
        System.out.println("3) Utazások száma hónaponként:");
        for (String honap : statisztika.keySet()) {
            System.out.printf("   %s.hó : %d utazás\n", honap, statisztika.get(honap));
        }

        // --- 4. feladat ---
        System.out.println("4) Többször szereplő vezetéknevek:");
        TreeMap<String, Integer> vezNevek = new TreeMap<>();
        for (Utazas u : utazasok) {
            String vezNev = u.nev.split(" ")[0];
            if (!vezNevek.containsKey(vezNev)) {
                vezNevek.put(vezNev, 1);
            } else {
                int db = vezNevek.get(vezNev);
                vezNevek.put(vezNev, db+1);
            }
        }

        System.out.print("   ");
        for (String vezNev : vezNevek.keySet()) {
            if (vezNevek.get(vezNev) > 1) {
                System.out.printf("%s ", vezNev);
            }
        }
        System.out.println();

        // --- 5. feladat ---
        System.out.print("5) Ugyanazon a napon kettőnél több utazás: ");
        TreeMap<String, Integer> tobbszori = new TreeMap<>();
        for (Utazas u : utazasok) {
            if (!tobbszori.containsKey(u.datum)) {
                tobbszori.put(u.datum, 1);
            } else {
                tobbszori.put(u.datum, tobbszori.get(u.datum)+1);
            }
        }

        for (String tobbszoriDatum : tobbszori.keySet()) {
            if (tobbszori.get(tobbszoriDatum) > 2) {
                System.out.printf("%s(%d) ", tobbszoriDatum, tobbszori.get(tobbszoriDatum));
            }
        }
        System.out.println();

        // --- 6. feladat ---
        PrintWriter ki = null;
        try {
            ki = new PrintWriter(new File("szeged.txt"), "UTF-8");
            for (Utazas u : utazasok) {
                if (u.varos.equals("Szeged")) {
                    ki.printf("%s (%s %s)\r\n", u.nev, u.datum, u.idopont);
                }
            }
        } catch (Exception e) {
            throw new RuntimeException(e);
        } finally {
            if (ki != null) ki.close();
        }
        System.out.print("6) Szegedre utazók kiírva a szeged.txt fájlba");
    }

    private void betolt(String fajlnev) {
        Scanner be = null;
        try {
            be = new Scanner(new File(fajlnev), "UTF-8");
            while (be.hasNextLine()) {
                utazasok.add(new Utazas(be.nextLine()));
            }
        } catch (FileNotFoundException e) {
            throw new RuntimeException(e);
        } finally {
            if (be != null) be.close();
        }
    }

    public static void main(String[] args) {
        new Main();
    }
}
```
### utazok.csv
```
Kovács Péter;Győr;03.30;18:44
Szabó Anna;Budapest;03.10;21:40
Tóth János;Miskolc;03.28;21:42
Horváth Zsófia;Kecskemét;03.17;13:29
Kiss László;Sopron;03.20;23:21
Molnár Éva;Szeged;05.25;18:43
Varga Gábor;Győr;06.26;22:53
...
```
## Feladat
```
 Az utazok.csv fájl különböző utazások adatait (név, város, dátum, indulás)
 tartalmazza, pontosvesszővel elválasztva, utf-8 kódolással. NINCS fejléc!
 Hozzunk létre egy Utazok nevű projektet és oldjuk meg a következő feladatokat!

 0) Olvassuk be a fájl adatait egy megfelelő adatszerkezetbe,
    és jelenítsük meg a beolvasott adatok számát!.....................(2p)
 1) Határozzuk meg hány féle városba utaztak!.........................(1p)
    Ezek közül válasszunk ki egyet véletlenszerűen,...................(1p)
    és írjuk ki hányan utaztak abba a városba összesen!...............(1p)
 2) Írjuk ki a legkorábbi indulás időpontját!.........................(1p)
    Írjuk ki azt is, összesen hány utazás kezdődött délelőtt!.........(1p)
 3) Készísünk statisztikát a havonkénti utazások számából!............(2p)
 4) Írjuk ki azokat a vezetékneveket (első név) ABC sorrendben,
    amelyek egynél többször fordulnak elő!............................(2p)
 5) Írjuk ki azokat a napokat, ahol kettőnél többen utaztak!..........(2p)
 6) Írjuk ki a szeged.txt fájlba a mintának megfelelően
    a Szegedre utazók adatait!........................................(2p)

 Minta:
 0) 50 utazó adata beolvasva
 1) Összesen 9 városba utaztak
    Közülük egy véletlenszerűen kiválasztott: Pécs
    Ebbe a városba 7 utazó érkezett
 2) Legkorábbi indulás: 10:17
    Összesen 5 utazás kezdődött délelőtt
 3) Utazások száma hónaponként:
    03.hó : 13 utazás
    04.hó : 17 utazás
    05.hó : 9 utazás
    06.hó : 11 utazás
 4) Többször szereplő vezetéknevek:
    Kocsis Kovács Molnár Papp Szabó Tóth Varga 
 5) Ugyanazon a napon kettőnél több utazás: 04.10(3) 05.10(4) 06.11(3) 
 6) Szegedre utazók kiírva a szeged.txt fájlba

 szeged.txt:
 Molnár Éva (05.25 18:43)
 Barna Ádám (04.16 11:45)
 Kuncz László (06.27 23:37)
 Varga András (04.23 15:56)
```
