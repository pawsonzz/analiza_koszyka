# Analiza koszyka: reguły asocjacyjne (EconoShop Express)

Eksploracja danych, laboratorium 6. Dane syntetyczne: 7 968 paragonów sklepu osiedlowego z września i października 2026 r. (`dane/paragony.csv`, jeden wiersz = jeden produkt na paragonie).

| Plik | Co to jest |
|---|---|
| `koszyk.py` | funkcje: macierz koszyków, wsparcie, pewność, lift, Apriori, scenariusz zysku (TODO 1–3 do uzupełnienia) |
| `tests/test_koszyk.py` | testy (`pytest -q`); na start `7 failed`, po TODO 1–3 `7 passed` |
| `lab6_student.ipynb` | notatnik z zadaniami Z1–Z6 |
| `app.py` | dashboard Streamlit |
| `render.yaml` | wdrożenie na Render |
| `.devcontainer/` | konfiguracja GitHub Codespaces (Python 3.13 i biblioteki instalują się same) |
| `.github/workflows/testy.yml` | testy na GitHubie po każdym push |

## 1. Praca w GitHub Codespaces

1. **Code → Codespaces → Create codespace on main**; poczekaj na koniec instalacji.
2. W terminalu: `pytest -q`, uzupełnij TODO 1–3 w `koszyk.py`, aż zobaczysz `7 passed`.
3. Otwórz `lab6_student.ipynb` → **Select Kernel → Python 3.13** → uruchamiaj komórki po kolei (Shift + Enter).
4. `streamlit run app.py` → **Open in Browser** (albo zakładka PORTS → 8501).

## 2. Zapis zmian

Panel **Source Control** → opis → **Commit** → **Sync Changes** (albo `git add .`, `git commit -m "..."`, `git push`). Zakładka **Actions**: zielony znaczek = testy przechodzą.

## 3. Render

**New → Blueprint** → **Connect** przy tym repozytorium → **Deploy Blueprint**. Ręcznie: **New → Web Service**, Build `pip install -r requirements.txt`, Start `streamlit run app.py --server.port $PORT --server.address 0.0.0.0 --server.headless true`, zmienna `PYTHON_VERSION` = `3.13.5`. Usługa Free usypia po 15 min bez ruchu.

## 4. Rekomendacja dla kierownika sklepu

Dwie reguły do działania to **makaron → sos pomidorowy** (207 paragonów, pewność 0,312, lift 5,97) oraz **pieluchy → chusteczki nawilżane** (108 paragonów, pewność 0,307, lift 6,33). 

Sos pomidorowy ustawiamy obok makaronu **bez rabatu**, bo 207 z 664 klientów z makaronem i tak kupuje oba produkty: rabat 20% dałby według założeń −273,80 zł wobec +47,99 zł z samej ekspozycji i zwracałby się dopiero przy konwersji 90,6%. 
Chusteczki nawilżane wystawiamy przy pieluchach i proponujemy je 244 klientom, którzy kupili pieluchy bez chusteczek (np. w zestawie). 

**Nie polecam** stałych działań wokół reguły węgiel drzewny → kiełbasa (lift 8,25, 119 paragonów), bo to pułapka sezonowości: we wrześniu było 117 paragonów i lift 6,94, a w październiku tylko 2 paragony (lift 2,10), więc w styczniu reguła będzie nieaktualna. Nie opieram się też na regułach z chlebem lub mlekiem po prawej stronie (np. mleko → chleb: lift 1,07), bo to tylko popularność następnika. 

Test A/B: przez 4 tygodnie połowa sklepów (wylosowana) dostaje nowe ustawienie (sos przy makaronie, chusteczki przy pieluchach), a druga połowa jest grupą kontrolną. Porównujemy odsetek paragonów z makaronem, na których jest sos (dziś 31,2%), oraz marżę na paragon, i wdrażamy zmianę we wszystkich sklepach tylko wtedy, gdy różnica jest wyraźna i nie wynika z przypadku.

Aplikacja: **https://TWOJA-NAZWA.onrender.com** (← wstaw swój adres z Render).
