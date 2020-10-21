
# GNU make Dash DocSet

This is a copy of [GNU make HTML Manual](https://gnu.org/software/make/manual) adapted for Dash.

Dash is a macOS documentation viewer with offline capabilities. As you can see,
it's pretty easy to extend and add custom HTML based documentation into the app.

## How to use

1. Clone this repository with submodules:
  - `git clone https://github.com/pvtmert/gnumake.docset`

2. Create/initialize indexes using `make -C Contents clean index`

3. Add this to the Dash;
  - Open Dash,
  - Open Preferences (`CMD+,`),
  - In the DocSets tab,
  - Click little `+` button on lower-left corner,
  - Select `Add local Docset`,
  - Point to this repository.

## How is this repo works.

Firstly, when cloned, thanks to the magic of [`Info.plist`](Contents/Info.plist)
and `.docset` extension, Dash will be handling it if installed properly.

I did not want to clutter up the repository by including SQLite3 indexes. Also,
they might change in the future in the actual HTML documents

The upstream repository added as a submodule, to update it, you can execute
`make -C Contents update`.

- Use `make -C Contents clean` to remove index.
- Use `make -C Contents index` to create index.

> Note: index re-creation is needed after updating the source documentation.

## How index getting created

Firstly, we create `Contents/Resources/Documents` directory.
Then download & extract `.tar.gz` HTML archive of GNU make documentation into
that directory.

Then using `grep` with recursive `-r` option to extract HTML title tags.
The output of `grep` then piped into the `sed` tool to remove HTML `<title>`
tags.

The final and filtered output is looped through `while read ...` part to
generate SQL `INSERT` statements and piped to `sqlite3`.

## Caveats

- The `icons` rule will download PNG icons from GNU's website.
- The `update` rule also downloads some CSS stylesheets from GNU and edits the
downloaded HTML documentation source to point to them.

## Reporting Bugs...

As in any software, bugs are highly probable and scared of light. If you find
them, please open an issue with a list of steps to reproduce.

If you also fix them, pull-requests are very-much welcome!

## License

Do whatever you want. I neither own Dash or GNU make. Keep in mind that
Dash is neither free-software nor open-source. It has a trial period and nags
you with your time after that period ends. However, you can use it as long as
you want.

> You should consult GNU as the `make` is licensed with GPL.
