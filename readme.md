# Open Local Benchmark

The scope of this project is to interface with a locally hosted LLM using LM Studio and evaluate key metrics using custom benchmark

Is it better to run a 14B Q4 or a 7B Q8 or a 20B Q1?

Is it better to use ROCm runtime or Vulkan runtime?

Those are the questions that this open local benchmark is meant to answer

## Benchmarks

The code of the benchmark is meant to be open source

The benchmark themselves are the result of over one year of prompt collections, I designed them to have an accuracy metric that is more relevant to my workflows and I'm never sharing them. This is to prevent the benchmark to eventually make into new model training data and making the benchmarks useless. I really do not want to make new benchmarks.

You can make your own benchmark as json files, I designed a system prompt that seems to work well and simplifes answer detection using html tags.

#### TODO

There are tasks that are not measured, like making python programs that is something I do a lot, but it requires a more complex harness and for the MVP I don't do it. 

## Scoring

I calculate three scores.

- green is structure, it measures when the LLM uses the correct tags and understand the system prompt and the task.

- orange is match, it measures when the LLM answers each question. This measures when the LLM doesn't gets confused, and E.g. start inventing more answers or forgets to give answers. it happened that a benchmark of 320 questions, the LLM stoped at 1653 questions, this is what matching measures.

- cyan is accuracy. it measures when the LLM gives a correct answer. It's measured by counting how many mismatching characters are in the answer.

I calculate two speeds

- Question is usually called prefill, or time to first token. It's system prompt+benchmark

- Answer is the generation speed

## Metrics

- Accuracy: straightforward. 100% is maximum, 0% is minimum. I assign pretty harsh penalties to mistakes, and score is always capped, a benchmark cannot get less than 0%.

- Yapping: (work in progress) verbose models will take longer to answer, making the yapping a meaningful metric. Do the extra character, combined with speed and accuracy make it worth it?

- Speed: there are two speeds Token and Char. interestlingly the tokenizer makes it so that different model have different compression rate, and so model that have similar speed in token, but stronger compression, will be able to output more characters per second. I compute both.

# ROCm vs Vulkan

Small differencies in accuracy are expected, I run each benchmark twice to compute standard deviation and is shown as size of the rectangles, both vertical and horizontal.

## Gemma 2 2B Q5

This is an old small model. Here Vulkan has a slight advantange over ROCm.

This small model I used to calibrate the benchmarks, the green score, structure shows that even small, old models understand how to answer the benchmark with html tags reliably, they even have a decent ability of matching most questions with answers.

As a small model it

Context is middling at 8000T, and 10 benchmarks fail as they are too big.

![](/images/gemma2-model_accuracy_vs_speed_tps.png)

## Qwen 3 14B Q4

This is a much higher performance model, here ROCm wins out in speed meaningfully, both in question and in answer.

![](/images/qwen14B-Q4-rocm-think.png)


# Quantization vs Parameters

Is it better to run an higher quant of a small model, or a lower quant of an higher model?




