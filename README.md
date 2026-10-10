# Building a domain-specific harness

Slides from my talk at **AAIF Pune Connect #3 · Agentic AI Day**, 10 October 2026, at Coditas, Pune.

**▶ View the slides: https://varr.run/talks/domain-harness/**

Everyone uses the same AI models. What you build around the model, the harness, decides how good the results are. This talk shows how to build one for your own field, using a Jyotish (Indian astrology) harness as the worked example. It follows one question, "When will I become a millionaire?", through five levers: context, tools, checks, rules and memory.

## Run it locally

It's one HTML file with no build step and no network calls. Fonts and images are in this folder.

```
npx http-server . -p 8181
```

Then open http://localhost:8181.

| Key | Action |
|---|---|
| → · space · click right | next slide |
| ← · click left | previous slide |
| G | grid of all slides |
| F | fullscreen |
| swipe | next / previous on a phone |

## Links

- JyotishBench, which AI reads a birth chart best: https://jyotishbench.fyi
- The harness I built live at NomadCoder, July 2026: https://github.com/varun1505/nomadcoder-harness-engg
- HERMES paper cited on slides 7–8: Jin, Li, Kuang, Wang, *Harness Engineering for Software Engineering via Modular Executable Dev-Primitives*, arXiv:2610.07832

## Credits

Fonts: Instrument Serif and Inter, both under the SIL Open Font License.

Varun Srinivas · CTO & Chief AI Officer, Coditas
