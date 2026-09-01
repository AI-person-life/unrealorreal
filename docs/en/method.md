# The method

On a machine with no graphics card, worked by one person.

🇭🇺 [Magyarul](../hu/modszer.md)

## 1. Canon first, images second

Before a single image exists, write down **who this character is**: the features
that never change, and the ones that may. It's a boring step, which is exactly why
people skip it.

Without canon, images drift relative to each other, and you end up with no
recognisable figure — just a collection of similar-looking people.

Ours is machine-readable too: one JSON per character holding the fixed features,
the prohibited depictions, and the line the character says about itself.

## 2. Train in the cloud, generate locally

LoRA training is the only step that needs serious compute — that's the part you
buy, and it accounts for the project's entire cost. Everything else runs locally.

**The last epoch is not the best one.** Download several and compare them against
the same test prompt. The choice is made by eye.

## 3. Many images, few survivors

65 usable images out of 488. That isn't waste, it's the nature of the work: since
generation costs nothing, selection becomes the bottleneck.

Pick from your first ten and you'll be publishing the model's mistakes.

## 4. Post-process where it's cheaper

Anything a single image-editing command fixes should not be argued out of the
model. Mirroring, cropping, colour correction are trivial operations — talking a
model into the same result takes hours and isn't deterministic.
