# kaggle-worker-queue

Task queue for the Kaggle image-generation worker (`bubu12-worker`).

- The worker kernel polls `queue.json` via its raw URL every 30 seconds:
  `https://raw.githubusercontent.com/charlock/kaggle-worker-queue/main/queue.json`
- Each job is an object with `id` plus optional `prompt`, `negative_prompt`,
  `seed`, `num_images`, `num_inference_steps`, `guidance_scale`,
  `ip_adapter_scale`. A job without `prompt` uses the worker template default
  (short yier_bubu prompt, 69 CLIP tokens, yier-only IP-Adapter reference).
- Jobs are processed in place by a single long-running kernel; when no new
  job arrives for 30 minutes the worker exits and results are fetched with
  `kaggle kernels output`.
- Do not edit `queue.json` by hand while a worker is running; use the
  kaggle-platform CLI `enqueue` command and push the updated file.
