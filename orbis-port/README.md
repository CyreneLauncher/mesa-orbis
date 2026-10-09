# Orbis Mesa port

This checkout retains the pinned upstream Mesa source and the complete imported
launcher patch history. `patches/*.patch` is the authoritative ordered port series.
The source checkout is a clean base; build_mesa.py applies the series to a separate
revision-pinned build checkout and reverses it after building. Do not apply it twice.

Build recipe: `python3 build_mesa.py --help`. Set ORBIS_LAUNCHER_ROOT to the integration
checkout when it is not the sibling orbis-launcher. Integration owns dependency/SDK
pins, output locations, PRX packaging and audit tooling. Port changes belong here;
update the launcher's repository pin after committing them.
