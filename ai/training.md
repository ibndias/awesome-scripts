# Training LLM
Use [Axolotl](https://github.com/axolotl-ai-cloud/axolotl) to train your LLM. At the time of this writing, nothing is better than that.
TODO: Include training script here.

# Upload model to huggingface quickly
Usually after you train your model, there are many checkpoints. If you are a ~~model hoarder~~ researcher, you can keep the model for each checkpoint and upload that as different revision (a.k.a. branch).

Go to your model output directory, usually contains the checkpoints:
```sh
root@7f99639d90f0:/workspace/axolotl/my_cool_model# ls
added_tokens.json  checkpoint-15124  checkpoint-22686  checkpoint-30248  checkpoint-3781  config.json  special_tokens_map.json  tokenizer_config.json
checkpoint-11343   checkpoint-18905  checkpoint-26467  checkpoint-34029  checkpoint-7562  merges.txt   tokenizer.json           vocab.json
```

Remove the unused monstrous file:
```sh
root@7f99639d90f0:/workspace/axolotl/my_cool_model# rm -rf  checkpoint-*/global_*
```

Upload all of the checkpoints:
```sh
for ckpt in checkpoint-*; do huggingface-cli upload myrepo/my_cool_model ./$ckpt . --private --revision ${ckpt#checkpoint-}; done
```
