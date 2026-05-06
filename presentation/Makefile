TEX = pdflatex
FLAGS = -output-directory=$(OUTPUT_DIR) -shell-restricted -no-shell-escape
AFLAGS = -interaction=nonstopmode
OUTPUT_DIR = output
FILE_NAME = presentation

.PHONY: all check dir index compile error install gitignore clean

all: check dir compile

dir:
	@mkdir -p $(OUTPUT_DIR)

check:
	@if [ ! -f $(FILE_NAME).tex ]; then \
		echo "Error: $(FILE_NAME).tex not found!"; \
		exit 1; \
	else \
		echo "$(FILE_NAME).tex"; \
	fi

index:
	@# Manual option:
	@if [ -f $(OUTPUT_DIR)/$(FILE_NAME).nlo ]; then \
		makeindex $(OUTPUT_DIR)/$(FILE_NAME).nlo -s nomencl.ist -o $(OUTPUT_DIR)/$(FILE_NAME).nls; \
	# else \
	# 	echo "No .nlo file found. Skipping indexing."; \
	fi

	@# Dynamically handle .nlo files for indexing
	@# find $(OUTPUT_DIR) -name "*.nlo" -exec makeindex {} -s nomencl.ist -o {}.nls \; 2>/dev/null \
	# 	|| echo "No .nlo files found. Skipping indexing."

compile: index
	@$(TEX) $(FLAGS) $(AFLAGS) $(FILE_NAME).tex >/dev/null
	@$(TEX) $(FLAGS) $(AFLAGS) $(FILE_NAME).tex >/dev/null

error: index
	$(TEX) $(FLAGS) $(FILE_NAME).tex

install:
	# Install the required packages
	sudo apt update -y
	sudo apt install -y \
		texlive-xetex \
		texlive-latex-recommended \
		ttf-mscorefonts-installer \
		texlive-fonts-extra \
		texlive-publishers \
		fonts-stix
	# Check if font is installed
	# sudo fc-cache -f && fc-match $(FONT_NAME) \
	# 	|| echo "$(FONT_NAME) font not found"

gitignore: dir
	@# Create .gitignore file on output directory if it doesn't exist
	@if [ ! -f $(OUTPUT_DIR)/.gitignore ]; then \
		echo "*\n!.gitignore\n!$(FILE_NAME).pdf" > $(OUTPUT_DIR)/.gitignore; \
		echo ".gitignore file created in $(OUTPUT_DIR) directory"; \
	else \
		echo ".gitignore file already exists in $(OUTPUT_DIR) directory"; \
	fi

clean:
	rm -rf \
		$(OUTPUT_DIR)/*.log \
		$(OUTPUT_DIR)/*.aux \
		$(OUTPUT_DIR)/*.lof \
		$(OUTPUT_DIR)/*.tof \
		$(OUTPUT_DIR)/*.tot \
		$(OUTPUT_DIR)/*.tol \
		$(OUTPUT_DIR)/*.toc \
		$(OUTPUT_DIR)/*.out \
		$(OUTPUT_DIR)/*.ilg \
		$(OUTPUT_DIR)/*.nls \
		$(OUTPUT_DIR)/*.nlo \
		$(OUTPUT_DIR)/*.fls \
		$(OUTPUT_DIR)/*.nav \
		$(OUTPUT_DIR)/*.snm \
		$(OUTPUT_DIR)/*.vrb \
		$(OUTPUT_DIR)/*.fdb_latexmk
