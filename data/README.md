# External dataset

The data used in this project is from [LinkedIn Job Postings (2023–2024)](https://www.kaggle.com/datasets/arshkon/linkedin-job-postings) on Kaggle, credited to Arsh Koneru and collaborators.

Download the dataset from the source and put these three CSV files in this `data/` directory:

```text
data/
├── README.md
├── postings.csv
├── companies.csv
└── employee_counts.csv
```

The original Kaggle download may place the files in subdirectories. Move or copy them into `data/` and retain their exact filenames.

The notebook expects these paths relative to the repository root, not Deepnote-specific `/work/` paths. Dataset CSVs are excluded from version control in `.gitignore` so the repository does not redistribute the raw data.

The Kaggle listing provides the original data description and applicable license. Review that listing before independent reuse or redistribution.
