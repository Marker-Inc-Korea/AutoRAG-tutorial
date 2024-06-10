# AutoRAG-template
Template for a new AutoRAG project


# Installation

```bash
pip install -r requirements.txt
```

# Running the project

1. Download dataset to data folder.
2. Make `.env` file using `.env.template` file.
3. Run evaluator with the following command.
```bash
python3 main.py --config ./config/tutorial.yaml
```
3. Check the result in the benchmark folder.

You can check the example config file at config folder.

And you can specify qa data path, corpus data path, and project dir if you want.


# Run dashboard

```bash
autorag dashboard --trial_dir ./benchmark/0
```

