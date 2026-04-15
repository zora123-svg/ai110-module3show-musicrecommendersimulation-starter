# Reflection: Comparing User Profile Outputs

---

## Pair 1 — Happy Pop (Original Weights) vs. Happy Pop (Adjusted Weights)

**What changed:**
With the original weights, "Gym Hero" ranked #2 and "Rooftop Lights" ranked #3. After halving the genre weight (from +2.0 to +1.0) and doubling the energy weight (from max 1.0 to max 2.0), those two songs swapped places.

**Why it makes sense:**
Gym Hero is a pop workout anthem tagged as "intense." Rooftop Lights is an indie pop song tagged as "happy." A listener asking for happy pop music clearly wants the second one, not the first. The original system gave Gym Hero a 2-point bonus just for having the exact label "pop," which was enough to override the fact that it had the wrong mood entirely.

Think of it this way: imagine asking a store clerk for "a happy birthday cake" and they bring you a chocolate cake because the bakery labels all chocolate cakes as "cake," while the actual birthday cake they have is labeled "celebration cake." The clerk matched the word "cake" and ignored "birthday." That is exactly what the original genre weight was doing.

Reducing the genre weight and rewarding energy fit more generously meant that Rooftop Lights — which is closer to the target energy of 0.8 and has the right mood — finally outscored Gym Hero. The rankings now match what a reasonable person would expect.

---

## Pair 2 — Happy Pop Profile vs. Adversarial High-Energy Sad Profile

**What changed:**
The happy pop profile produced a clean, intuitive top result: Sunrise City at #1, followed by Rooftop Lights. The adversarial profile — someone who wants high-energy (0.9) sad music — produced a list dominated by songs that matched the energy but completely ignored the sad mood. Storm Runner (intense rock) and Gym Hero (intense pop) both appeared near the top despite being nowhere near "sad."

**Why it makes sense:**
The sad songs in the catalog, like "Rainy Porch Blues," have low energy (around 0.4). A listener who wants sad music at high energy is describing something rare: think "Running Up That Hill" or a grieving anthem that still hits hard. The dataset simply does not have that kind of song. Because energy proximity is now worth up to 2.0 points, high-energy songs score well on that dimension regardless of mood. Since there is no sad + high-energy song to find, the system settles for the next best thing: high-energy songs with the wrong mood.

This is not a bug in the math — the math is doing exactly what it was told. The problem is that the scorer treats energy and mood as two completely separate questions, when in real music they are often deeply connected. A song labeled "sad" almost never has energy above 0.8. The two profiles reveal that the system works well when the user's preferences are internally consistent with how real music is made, and struggles when they are not.

---

## Pair 3 — Pop Fan vs. Jazz Fan

**What changed:**
The pop profile returned variety: two pop songs (Sunrise City and Gym Hero) competed for the top spots, with other songs filling in around them. The jazz profile always returned "Coffee Shop Stories" by Slow Stereo as #1, every single time, with no variation possible.

**Why it makes sense:**
There are two pop songs in the catalog and only one jazz song. A pop fan at least gets to see competition between genre-matching songs — the one with the better mood and energy wins. A jazz fan has no such competition. Coffee Shop Stories earns the genre bonus automatically and there is nothing else to challenge it. Even if Coffee Shop Stories has the wrong energy or a mismatched mood for that specific jazz listener, it will always be #1.

This is the dataset variety problem in plain view. A recommender system is only as good as the options it has to choose from. Giving the system better scoring logic does not help if there is only one jazz song to recommend. Any jazz fan who wants something energetic, or something instrumental, or something with a different mood than "relaxed," simply cannot get a good recommendation — not because the algorithm failed, but because the catalog has no alternatives to offer.
