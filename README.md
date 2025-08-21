# Webáruház vásárlói viselkedés előrejelzése – kiterjesztett feature engineering és modellezés

# Projekt áttekintés

Ez a projekt a webáruházban gyűjtött session- és logadatokra építve a felhasználói viselkedés (pl. vásárlási konverzió) előrejelzésére fókuszál.
A munka során több feladatrészben történt a feature engineering bővítése, különféle modellek kipróbálása, valamint a predikciók validációs beküldési fájlokba mentése.

# Adatok

Bemeneti fájlok:

input_session_target.csv – célváltozó (target) és Usage bontás (train/test/validation)

groupNum_all.csv – aggregált idő- és kattintás-alapú jellemzők

group0_shopID.csv, group1_referrer.csv, group2_type.csv, group3_linktype.csv, group4_category.csv, group5_product_type.csv, group6_brand.csv, group7_product.csv – kategóriánkénti és ritka attribútumokat tartalmazó fájlok

Session-szintű jellemzők:

Időbeli változók (pl. session kezdete, időeltérések statisztikái)

Termékekhez, kategóriákhoz, márkákhoz, shopID-hoz kötődő flag és időalapú jelzők

Dimenziócsökkentett jellemzők (PCA, KMeans komponensek)

# Feature engineering lépések

Alapváltozók: időbeli jellemzők, kattintás statisztikák.

Logisztikus regresszióval top 3 változó kiválasztása:

time_before_click_std

duration_std

time_before_click_max

Kiterjesztett feature space: több ezer változó a különböző group fájlokból.

Low-variability szűrés: alacsony varianciájú attribútumok elhagyása → több száz felesleges oszlop eltávolítása.

Top változók kiválasztása AUC alapján (pl. shopID és típus flag-ek, PCA és KMeans komponensek).

Kategória (category) alapú jellemzők bevonása és feature importance vizsgálata.

Végső feature set összeállítása (idő, shopID, type, category komponensekkel).

# Modellezés

Baseline: Logistic Regression (skálázott adatokkal).

Fejlettebb modellek:

Gradient Boosting Classifier (scikit-learn implementáció)

Fokozatos változóbeválogatás AUC-mutató alapján (wrapper jellegű feature selection)

Értékelés: ROC AUC mutató a train/test és validation halmazon.

# Kimenetek

Beküldési fájlok session-szintű előrejelzésekkel (CSV):

beadando_1.csv – baseline logisztikus regresszió top 3 feature-rel

beadando_3.csv – shopID + időbeli változók alapján logreg/GBM modell

beadando_4.csv – type kategória és wrapper feature selection (GBM modell)

beadando_5.csv – végső modell (GBM, feature importance alapján kiválasztott változókkal)

# Használt könyvtárak

pandas, numpy – adatelőkészítés

scikit-learn – StandardScaler, LogisticRegression, GradientBoostingClassifier, ROC AUC metrika

matplotlib – feature importance vizualizáció

# Következtetések

A baseline logisztikus regresszió mérsékelt eredményt (AUC ~0.60) adott.

A különböző csoportosított jellemzők (shopID, type, category) jelentősen növelték a predikciós teljesítményt.

A fokozatos feature selection javította a Gradient Boosting modellek teljesítményét (AUC ~0.65 körül).

A végső modell a különböző forrásokból kiválasztott top változókat kombinálva készült el, és CSV formátumban került beadásra.
