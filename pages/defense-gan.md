[Strona główna](../README.md)
# Defense GAN
Jak się okazało modele generatywne również bardzo dobrze sprawdzają się w scenariuszach obrony przeciwko atakom antagonistycznym.
W pracy „Defense-GAN: Protecting classifiers against adversarial attacks using generative models” [1] przedstawiona została koncepcja sieci nazwanej przez twórców jako Defense-GAN, która jest wyszkolona by napodstawie wejścia modelować dane bez występujących wantagonistycznych przykładach zakłóceń.
Dane wejściowe zanim zostaną przekazane do docelowego klasyfikatora przechodzą przez generator sieci GAN, który generuje dane zbliżone do wejściowych, ale nie zawierające szumu.

![Architektura sieci Defense-GAN [1]](../resources/defense-gan-model.png)  
*Architektura sieci Defense-GAN [1]*

Sieć GAN jest szkolona na niezakłóconych danych wejściowych w taki sposób, aby odszumiać antagonistyczne przykłady. Ta metoda obrony może być stosowa z dowolnym klasyfikatorem nie modyfikując struktury samego klasyfikatora ani procesu jego uczenia.
Dodatkowo może być wykorzystana przeciwko dowolnym atakom antagonistycznym, aproces szkolenia generatora w przeciwieństwie do treningu antagonistycznego nie zakłada znajomości algorytmu wykorzystywanego przez atakującego do generowania antagonistycznych przykładów. Metoda jest skuteczna przeciwko antagonistycznym atakom zarówno czarno jak i biało skrzynkowych.

## Źródła
* [[1] Defense-GAN: Protecting classifiers against adversarial attacks using generative models](https://arxiv.org/pdf/1805.06605.pdf)
