# Core of a 3-Player TU Game — Interactive 3D Visualizer

An interactive web app for exploring the **core** of a three-player cooperative game with transferable utility (TU).

Move the sliders to set the worth of every coalition, and the 3D plot updates live:

- highlighted **axes** for players A, B and C (the plot shows non-negative payoffs only),
- the **efficiency plane** x_A + x_B + x_C = v(N),
- the six **constraint planes** xᵢ = v({i}) and xᵢ + xⱼ = v({i,j}), each in its own color,
- their **intersection lines** with the efficiency plane,
- the **imputation set** (dashed outline),
- the **core**, shaded on the efficiency plane,
- the **Shapley value** φ and the **nucleolus** ν, each with a check of whether it lies in the core.

The side panel reports whether the core is empty, a point, a segment or a polygon, lists its vertices, and checks the Bondareva–Shapley conditions for three players.

## The model

A TU game with players N = {A, B, C} is given by a characteristic function v that assigns a worth to each coalition:
v({A}), v({B}), v({C}), v({A,B}), v({A,C}), v({B,C}), v(N).

The core is the set of allocations x = (x_A, x_B, x_C) satisfying

| Condition | Inequality |
|---|---|
| Efficiency | x_A + x_B + x_C = v(N) |
| Individual rationality | xᵢ ≥ v({i}), i = A, B, C |
| Coalitional rationality | xᵢ + xⱼ ≥ v({i,j}) for each pair |

For n = 3, the core is non-empty if and only if

- v(N) ≥ v({A}) + v({B}) + v({C})
- v(N) ≥ v({i,j}) + v({k}) for every pair {i,j}
- 2·v(N) ≥ v({A,B}) + v({A,C}) + v({B,C})

### How the core is computed

On the efficiency plane, x_C = v(N) − x_A − x_B, so every inequality becomes a half-plane in (x_A, x_B). The app clips a polygon by the six half-planes (Sutherland–Hodgman), then lifts the result back to 3D. Degenerate cores (a point or a segment) are drawn as a marker or a thick line.

Only the non-negative orthant is drawn. The vertex table always reports the full core; if part of it has negative payoffs, the panel says so.

### Nucleolus

The nucleolus is the imputation that lexicographically minimises the vector of excesses e(S, x) = v(S) − x(S), sorted from largest to smallest. It is computed with the sequential linear programs of Maschler, Peleg and Shapley (1979): minimise the largest excess, fix the coalitions that cannot do better, and repeat. Each LP has only three variables, so it is solved exactly by enumerating vertices. The nucleolus is defined whenever the imputation set is non-empty, and it lies in the core whenever the core is non-empty.

## Presets

- Example with a non-empty core
- Symmetric game with a small core
- Majority game (empty core)
- Glove market (core is a single point)
- Additive game (core is a single point)
- Game with non-zero singleton worths

## Run it locally

It's a single static file. Open `index.html` in any modern browser — no build step, no server needed. (It loads Plotly.js from a CDN, so you need an internet connection.)

## Publish on GitHub Pages

1. Create a new repository on GitHub (e.g. `tu-game-core`).
2. Upload `index.html` and `README.md` to the root of the repository
   (*Add file → Upload files*, or with git:
   `git init && git add . && git commit -m "Initial commit" && git branch -M main && git remote add origin https://github.com/<user>/tu-game-core.git && git push -u origin main`).
3. In the repository, go to **Settings → Pages**.
4. Under **Build and deployment**, choose **Deploy from a branch**, select `main` and `/ (root)`, and click **Save**.
5. After a minute or so the app will be live at `https://<user>.github.io/tu-game-core/`.

## Controls

- **Sliders / number boxes** — set each coalition's worth. Typing a value outside the slider range extends the slider.
- **Legend** — click an entry to hide or show that constraint (plane and line together).
- **Look straight at the plane** — orients the camera along (1, 1, 1), so the efficiency plane faces you and the core appears as a flat 2D polygon, like the classic barycentric triangle picture.
- Drag to rotate, scroll to zoom, hover over core vertices to read coordinates.

## Tech

Plain HTML/CSS/JavaScript with [Plotly.js](https://plotly.com/javascript/) for the 3D rendering. Supports light and dark mode.
