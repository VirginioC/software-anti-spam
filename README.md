# Analisi e classificazione delle email per la rilevazione di SPAM

## Descrizione e obiettivi del progetto
Questo progetto, realizzato durante il Master in Data Science di Profession AI, si propone di sviluppare col linguaggio **Python** su ambiente **Google Colab**, una libreria software per l'analisi e la classificazione delle email, con l'obiettivo di identificare e comprendere meglio le email di tipo spam. Attraverso tecniche di NLP e Machine Learning, il software permette di:

1. **Addestrare un classificatore** per identificare le email SPAM.
2. **Individuare i topic principali** tra le email classificate come SPAM.
3. **Calcolare la distanza semantica** tra i topics ottenuti per valutare l'eterogeneità dei contenuti delle email SPAM.
4. **Estrarre dalle email NON SPAM** le informazioni sulle Organizzazioni menzionate.

Le informazioni sulle tendenze, i contenuti e i comportamenti delle email SPAM  possono essere utilizzate per migliorare la sicurezza delle comunicazioni aziendali e perfezionare i filtri anti-spam.

## Struttura del progetto
Il progetto è organizzato nelle seguenti fasi:

1. **Analisi descrittiva**:
   - Studio del dataset **`spam_dataset.csv`** (incluso nel repository), con colonne `text` (email) e `label_num` (0 = ham, 1 = spam).
   - Identificazione di consistente **sbilanciamento del dataset** (71 % ham).

2. **Preprocessing dei dati**:
   - Pulizia del testo: conversione in lowercase, rimozione di punteggiatura e numeri, rimozione stopwords, lemmatizzazione e rimozione spazi bianchi extra.
   - Creazione dell'array contenente i messaggi testuali "puliti" e dell'array con la variabile target.

3. **Classificazione**:
   - Per effettuare la **classificazione binaria** viene costruita una pipeline contenente vectorizer **TF-IDF** e modello ML, addestrata e valutata tramite **K-Fold Cross-Validation** (5 fold) con metriche: **accuracy**, **precision**, **recall** e **f1-score**.
   - Modelli valutati:
      - **Logistic Regression**: utilizzo del parametro `class_weight="balanced"`
      - **Complement Naive Bayes**
      - **Support Vector Machines**: utilizzo del parametro `class_weight="balanced"`
      - **Random Forest**: utilizzo del parametro `class_weight="balanced"`

   - Il modello migliore è il **SVM**, vengono quindi ricavati i migliori iperparametri ottenendo metriche medie quasi perfette sul test set:
     
     ```python
      Testing Scores:
      ╒══════════╤════════════╤═════════════╤══════════╤══════╕
      │ Model    │   accuracy │   precision │   recall │   f1 │
      ╞══════════╪════════════╪═════════════╪══════════╪══════╡
      │ Best SVM │       0.99 │        0.97 │     0.99 │ 0.98 │
      ╘══════════╧════════════╧═════════════╧══════════╧══════╛
     ```
4. **Topic Modeling**:
   - Tokenizzazione di ciascuna email spam escludendo stopword, tag HTML, proprietà e attributi CSS, URL ed unità di misura.
   - Creazione del dizionario dei token (token, id) e conversione dei token nel formato **bag of Words** (id, count).
   - **Latent Dirichlet Allocation (LDA)** con valutazione di coerenza, identificando **5 topic principali**:
     
      - **Topic 0 (Farmaci)**: sfrutta il desiderio di privacy e risparmio per medicinali sensibili.
      - **Topic 1 (Commerciale)**: usa l'urgenza e le offerte limitate come leva psicologica.
      - **Topic 2 (Personale)**: fa leva sulle emozioni con riferimenti familiari e finanziari.
      - **Topic 3 (Software)**: sfrutta il desiderio di accedere a software costosi a prezzi ridotti.
      - **Topic 4 (Finanza)**: utilizza un linguaggio professionale per apparire legittimo.
   
   - Calcolo della **distance matrix**: le distanze semantiche hanno valori abbastanza elevati, indicando l'eterogeneità dei topic.
   - Analisi della distribuzione dei topic: il topic 0 è il più frequente (42.83 %).
  
5. **NER - Organizzazioni**
   - Task di **Named Entity Recognition (NER)** per estrarre tutte le organizzazioni presenti nelle email ham tramite modello **en_core_web_lg** di `spacy`.
   - Identificazione delle 10 organizzazioni più frequenti.
   - La **Enron Corporation** risulta essere, in maniera netta, l'organizzazione più menzionata.
  
## Tecnologie utilizzate
- **Linguaggio**: Python
- **Ambiente di sviluppo**: Google Colab (Jupyter Notebook)
- **Librerie**:
   - `pandas`
   - `numpy`
   - `matplotlib`
   - `seaborn`
   - `scikit-learn`
   - `nltk`
   - `tabulate`
   - `gensim`
   - `spacy`

## Utilizzo  
1. Scarica o clona il repository.
2. Apri il file `software_anti-spam.ipynb` su Google Colab o altri ambienti compatibili con Jupyter Notebook.
3. Esegui il codice passo-passo per ottenere i risultati.
4. Il dataset utilizzato (`spam_dataset.csv`) è incluso nel repository.

## Autore
[Virginio Cocciaglia](https://github.com/VirginioC)

---
