TEX = pdflatex -interaction=nonstopmode -halt-on-error
PUBLIC_TEX = xelatex -interaction=nonstopmode -halt-on-error
FIGURE_TEX = xelatex -interaction=nonstopmode -halt-on-error
BIB = bibtex
PREPRINT = dashvmc-preprint
CAMERA_READY = dashvmc-camera-ready
PY = python
FIGURE_FONT = ../docs/assets/fonts/AlphaLyrae-Medium.ttf

all: preprint camera-ready

preprint: $(PREPRINT).pdf

camera-ready: $(CAMERA_READY).pdf

figures/pipeline.pdf: ../docs/pipeline.tex $(FIGURE_FONT)
	$(PY) -c "import os; os.makedirs('figures', exist_ok=True)"
	cd ../docs && $(FIGURE_TEX) pipeline.tex
	$(PY) -c "import shutil; shutil.copy('../docs/pipeline.pdf', 'figures/pipeline.pdf')"

CONTINUATION_FRAMES = \
	../analysis/2026-07-21_counterfactual_spike/shared_context/frame_000003.png \
	../analysis/2026-07-21_counterfactual_spike/idle/frame_000000.png \
	../analysis/2026-07-21_counterfactual_spike/idle/frame_000004.png \
	../analysis/2026-07-21_counterfactual_spike/jump/frame_000000.png \
	../analysis/2026-07-21_counterfactual_spike/jump/frame_000004.png \
	../analysis/2026-07-21_counterfactual_spike/jump/frame_000012.png

figures/continuation.pdf: ../docs/continuation.tex $(FIGURE_FONT) $(CONTINUATION_FRAMES)
	$(PY) -c "import os; os.makedirs('figures', exist_ok=True)"
	cd ../docs && $(FIGURE_TEX) continuation.tex
	$(PY) -c "import shutil; shutil.copy('../docs/continuation.pdf', 'figures/continuation.pdf')"

$(PREPRINT).pdf: $(PREPRINT).tex main.tex references.bib fairpreprint.cls figures/pipeline.pdf figures/continuation.pdf
	$(PUBLIC_TEX) $(PREPRINT)
	$(BIB) $(PREPRINT)
	$(PUBLIC_TEX) $(PREPRINT)
	$(PUBLIC_TEX) $(PREPRINT)

$(CAMERA_READY).pdf: $(CAMERA_READY).tex main.tex references.bib neurips_2026.sty figures/pipeline.pdf figures/continuation.pdf
	$(TEX) $(CAMERA_READY)
	$(BIB) $(CAMERA_READY)
	$(TEX) $(CAMERA_READY)
	$(TEX) $(CAMERA_READY)

clean:
	$(PY) -c "import os; bases=['$(PREPRINT)','$(CAMERA_READY)','main']; suffixes=['.aux','.bbl','.blg','.log','.out','.pdf','.fls','.fdb_latexmk','.synctex.gz']; files=[base + suffix for base in bases for suffix in suffixes] + ['figures/pipeline.pdf','figures/continuation.pdf']; [os.remove(f) for f in files if os.path.exists(f)]"

.PHONY: all preprint camera-ready clean
