# Constitutional AI

This folder has some scripts used for generating Constitutional AI datasets based on anthropic's HH prompts. These pre-generated datasets are as follows:

- 📖 Constitutional AI dataset https://huggingface.co/datasets/HuggingFaceH4/cai-conversation-harmless
- 📖 Grok-style CAI dataset https://huggingface.co/datasets/HuggingFaceH4/grok-conversation-harmless

You can generate your own! Run the cai dataset generation script to debug

```
python examples/constitutional-ai/generate_dataset.py --push_to_hub
# use a different constitution
python examples/constitutional-ai/generate_dataset.py --push_to_hub --constitution_path="examples/constitutional-ai/constituion_grok.json"
```

Then to generate the whole dataset (it takes a while: runs for 1h 37min when using 64 GPUs):
```
python examples/constitutional-ai/generate_dataset.py --push_to_hub --max_samples=-1 --instances=8
python examples/constitutional-ai/generate_dataset.py --constitution_path="examples/constitutional-ai/constituion_grok.json" --push_to_hub --max_samples=-1 --instances=10
```
```
(.venv) costa@login-node-1:/fsx/costa/tgi-swarm$ python examples/constitutional-ai/generate_dataset.py --push_to_hub --max_samples=-1 --instances=6
None of PyTorch, TensorFlow >= 2.0, or Flax have been found. Models won't be available and only tokenizers, configuration and file/data utilities can be used.
Map: 100%|█████████████████████████████████████████████████████████| 42537/42537 [00:01<00:00, 32074.71 examples/s]
Map: 100%|███████████████████████████████████████████████████████████| 2312/2312 [00:00<00:00, 26755.85 examples/s]
running sbatch --parsable slurm/tgi_1705622089_tgi.slurm
running sbatch --parsable slurm/tgi_1705622089_tgi.slurm
running sbatch --parsable slurm/tgi_1705622089_tgi.slurm
running sbatch --parsable slurm/tgi_1705622089_tgi.slurm
running sbatch --parsable slurm/tgi_1705622089_tgi.slurm
running sbatch --parsable slurm/tgi_1705622089_tgi.slurm
Slurm Job ID: ['1188241', '1188242', '1188243', '1188244', '1188245', '1188246']
📖 Slurm Hosts Path: slurm/tgi_1705622089_host_tgi.txt
✅ Done! Waiting for 1188241 to be created                                                                         
✅ Done! Waiting for 1188242 to be created                                                                         
✅ Done! Waiting for 1188243 to be created                                                                         
✅ Done! Waiting for 1188244 to be created                                                                         
✅ Done! Waiting for 1188245 to be created                                                                         
✅ Done! Waiting for 1188246 to be created                                                                         
✅ Done! Waiting for slurm/tgi_1705622089_host_tgi.txt to be created                                               
obtained endpoints ['http://26.0.161.103:10613', 'http://26.0.173.246:38397', 'http://26.0.162.46:26736', 'http://26.0.161.138:49305', 'http://26.0.161.78:5552', 'http://26.0.161.153:30709']
⡿ Waiting for http://26.0.161.103:10613 to be reachable
Connected to http://26.0.161.103:10613
✅ Done! Waiting for http://26.0.161.103:10613 to be reachable                                                     
⣽ Waiting for http://26.0.173.246:38397 to be reachable
Connected to http://26.0.173.246:38397
✅ Done! Waiting for http://26.0.173.246:38397 to be reachable                                                     
⣟ Waiting for http://26.0.162.46:26736 to be reachable
Connected to http://26.0.162.46:26736
✅ Done! Waiting for http://26.0.162.46:26736 to be reachable                                                      
⣽ Waiting for http://26.0.161.138:49305 to be reachable
Connected to http://26.0.161.138:49305
✅ Done! Waiting for http://26.0.161.138:49305 to be reachable                                                     
⣽ Waiting for http://26.0.161.78:5552 to be reachable
Connected to http://26.0.161.78:5552
✅ Done! Waiting for http://26.0.161.78:5552 to be reachable                                                       
⣽ Waiting for http://26.0.161.153:30709 to be reachable
Connected to http://26.0.161.153:30709
✅ Done! Waiting for http://26.0.161.153:30709 to be reachable                                                     
Endpoints running properly: ['http://26.0.161.103:10613', 'http://26.0.173.246:38397', 'http://26.0.162.46:26736', 'http://26.0.161.138:49305', 'http://26.0.161.78:5552', 'http://26.0.161.153:30709']
✅ test generation
✅ test generation
✅ test generation
✅ test generation
✅ test generation
✅ test generation
running sudo docker run -d -p 60929:60929 --network host -v $(pwd)/slurm/tgi_1705622089_load_balancer.conf:/etc/nginx/nginx.conf nginx
running sudo docker logs 7c0bf1afddcaac304d3c1f104f8f66e787646561095ed27c851adf4a9afb5c2e
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
🔥 endpoint ready http://localhost:60929
WARNING: the first generation might hang a bit because of the multi-turn chat and long context.
 63%|███████████████████████████████████████████▊                          | 28054/44849 [1:24:25<12:58, 21.58it/s]
 ```

after a while the output is as follow. Note the tokens per second seems slow but it is because of the multi-turn chat and long context.

```
100%|██████████████████████████████████████████████████████████████████████| 44849/44849 [1:37:06<00:00,  7.70it/s]
Overall Tokens per Second: 2471.9039087752
```



## Advanced: generate system chats

Sometimes, if you have different models, you may need to re-generate system chats again. You can do it with the following command:

```
python examples/constitutional-ai/generate_system_chat.py --debug_endpoint="https://api-inference.huggingface.co/models/mistralai/Mistral-7B-Instruct-v0.1"
python examples/constitutional-ai/generate_system_chat.py --constitution_path="examples/constitutional-ai/constituion_grok.json" --debug_endpoint="https://api-inference.huggingface.co/models/mistralai/Mistral-7B-Instruct-v0.1"
```
```

Inspect the generated chats in the `examples/constitutional-ai/exps` folder and make modifications if needed.