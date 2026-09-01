# What we got wrong

This is the most useful page in the repository. All five failures are **silent**:
you don't get an error message, you get a bad result you don't know is bad.

🇭🇺 [Magyarul](../hu/hibak.md)

---

## 1. Image models don't understand negation

Asking for something *not* to be in the image is how you summon it. The model
weights the words; it doesn't evaluate the logical operator.

```
❌  "no tattoo on the right arm"                           → 0 hits out of 4
✅  "turned so her tattooed LEFT arm is nearest the camera" → 4 hits out of 4
```

**The pattern:** don't forbid — arrange the composition so the question never
comes up. Side can be fixed afterwards by mirroring (`convert -flop`), which is
a lossless operation anyway.

| forbidden it | told it what we wanted |
|---|---|
| ![Tattoo on the wrong arm](../kepek/tagadas-elotte.jpg) | ![Tattoo on the canonical arm](../kepek/tagadas-utana.jpg) |

*Same request, two phrasings. Canon puts the tattoo on her left arm — the
prohibiting version produced exactly the opposite.*

## 2. Download order is not epoch order

When picking between training epochs, filename order misleads you. Rely on it and
you'll test the wrong model without ever knowing.

**The reliable source is the file's own header:**

```python
# a safetensors header is JSON, prefixed by its length as a little-endian uint64
import json, struct
with open(path, "rb") as f:
    n = struct.unpack("<Q", f.read(8))[0]
    header = json.loads(f.read(n))
print(header["__metadata__"].get("ss_epoch"))
```

This saved us six unnecessary manual downloads before a deadline.

## 3. The click that fails silently

On a web-based training interface, the model-selection checkbox was a **0×0 pixel
hidden input**. Neither coordinate clicks nor element references caught it. The
interface reported no error — nothing simply got selected.

**What worked:** clicking the `label`, not the hidden input.

**The general lesson:** when automating, verify the *result*, never the click's
own feedback. Silent failures are the expensive kind — they don't stop, they
carry on with bad data.

## 4. Our side-detection script wasn't trustworthy

We tried to decide "which arm is it on" programmatically. It didn't work. What did
work: looking, with arm-strip crops at 380–400 px wide, 12 images per contact
sheet. A 230 px sheet wasn't enough.

Sometimes the human eye is the cheaper instrument.

## 5. Cloud LLMs are unusable for creative Hungarian

We tried several hosted models for Hungarian brand voice. They break the grammar,
and other languages bleed into the output. For short factual work — log analysis,
summarising, filtering — they're excellent and cheap. For creative writing in a
smaller language, they are not.

**A related trap:** "thinking" models return an **empty response** under a low
token budget, because the reasoning consumes the allowance. The failure doesn't
surface as an error — it surfaces as an empty field.

```
/api/generate        → "think": false
/v1/chat/completions → "reasoning_effort": "none"
```

## 6. Don't let a model do the matching

Our news pipeline asked one model call to filter a list of 66 headlines *and*
write four fields for each selected item. It started mismatching rows — pairing a
Hungarian headline with a completely unrelated source link. Not hallucination:
it lost track inside a long list.

**The fix was not a better prompt — it was a different shape.** Two stages: one
call selects indices only, then *one call per article* writes the text. Nothing
to mix up when there's no list.

We also kept a verification step that drops any item whose returned title doesn't
match the article at that index. It costs the occasional dropped story. Publishing
a correct headline over the wrong link costs credibility.
