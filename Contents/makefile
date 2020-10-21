#!/usr/bin/env make

INDEX  := ./Resources/docSet.dsidx
REMOTE := http://www.gnu.org/software/make/manual/make.html_node.tar.gz
ICON   := http://www.gnu.org/graphics/heckert_gnu.big.png
ICON   := http://www.gnu.org/graphics/heckert_gnu.transparent.png

index: $(INDEX)

./Resources/Documents:
	mkdir -p "$@"

update: ./Resources/Documents
	curl --compressed -#4Lf "$(REMOTE)" | tar -xzC "$@"
	find "$@" -iname "*.html" -exec \
		sed -i.old 's:/software/gnulib/manual.css:_manual.css:g' {} +
	curl -#4Lfo "$@/_manual.css" "http://www.gnu.org/software/gnulib/manual.css"
	sed -i .old 's:/style.css:_style.css:g' "$@/_manual.css"
	curl -#4Lfo "$@/_style.css" "http://www.gnu.org/style.css"
	sed -i .old 's:/reset.css:_reset.css:g' "$@/_style.css"
	curl -#4Lfo "$@/_reset.css" "http://www.gnu.org/reset.css"
	find "$@" -iname "*.old" -delete

icons: ../icon.png

../icon.png:
	curl -#4Lfo "$@" "$(ICON)"

../icon@2x.png: ../icon.png
	ln -s "$<" "$@"

clean:
	rm -vrf "$(INDEX)" "./Resources/Documents" "./Resources" "../icon@2x.png" "../icon.png"

define DB_INIT
CREATE TABLE searchIndex(id INTEGER PRIMARY KEY, name TEXT, type TEXT, path TEXT);
CREATE UNIQUE INDEX anchor ON searchIndex (name, type, path);
endef

export DB_INIT

$(INDEX): ./Resources/Documents icons update
	echo "$${DB_INIT}" | sqlite3 "$@"
	grep -roE '<title>(.+)</title>' "$<" | sed -e 's:<[/]*title>::g;s: (GNU make)::g' \
	| while IFS=: read file title; do \
		echo "INSERT OR IGNORE INTO searchIndex(name, type, path) \
			VALUES ('$${title}', 'Guide', '$$(basename "$${file}")');"; \
	done \
	| sqlite3 "$@"

