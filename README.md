# Resume Screening System

An end-to-end AI-powered resume screening pipeline that automatically extracts candidate signals, matches them against job descriptions, scores fit, and generates transparent explanations for hiring decisions.

## Features

- **Automated Resume Parsing**: Extracts skills, tools, and experience years from PDF resumes
- **Intelligent Job Matching**: Compares candidate qualifications against job requirements
- **Smart Scoring**: Implements improved scoring logic to prevent false positives
- **Explainable Results**: Provides detailed reasoning for each candidate evaluation
- **LangSmith Integration**: Full observability and tracing of the evaluation pipeline
- **Debugging Capability**: Demonstrates legacy bugs and corrections for educational purposes
- **Academic Quality**: Built for clarity, traceability, and portfolio presentation

## Requirements

- Python 3.8+
- Groq API Key
- LangChain API Key (for LangSmith tracing)
- PDF resume files

## Installation

### 1. Clone or Download the Project
```bash
cd resume-screening
```

### 2. Install Dependencies
```bash
pip install -r requirements.txt
```

### 3. Set Up API Keys
Open `main.ipynb` and update Cell 2 with your API keys:
```python
os.environ["GROQ_API_KEY"] = "your-groq-api-key-here"
os.environ["LANGCHAIN_API_KEY"] = "your-langchain-api-key-here"
```

Get your keys from:
- **Groq API**: https://console.groq.com/keys
- **LangChain API**: https://smith.langchain.com

### 4. Add Resume PDFs
Place your candidate resume PDFs in the `Resume/` directory:
```
Resume/
├── resume_strong.pdf
├── resume_average.pdf
└── resume_weak.pdf
```

## Usage

### Running the Notebook

1. Open `main.ipynb` in Jupyter or VS Code
2. Run cells sequentially from top to bottom:
   - **Cell 2**: Configure environment and API keys
   - **Cell 3-8**: Load libraries, initialize LLM, and set up prompts
   - **Cell 9-10**: Execute the pipeline on all candidates
   - **Cell 11**: View detailed results
   - **Cell 12**: Run debugging example to see bug vs. fix

### Expected Output

Each candidate receives:
- **Score**: 0-100 rating
- **Band**: Strong | Average | Weak
- **Matching Skills**: Skills present in both resume and job description
- **Missing Skills**: Required skills not found in resume
- **Recommendation**: Hiring decision rationale

Example:
```
========================================================================
Candidate: Strong
Score: 92 | Band: Strong
Matching Skills: ['Python', 'Machine Learning', 'NLP']
Missing Skills: []
Recommendation: Excellent fit. All required skills present with 5+ years...
```

## Project Structure

```
resume-screening/
├── main.ipynb              # Main Jupyter notebook with full pipeline
├── requirements.txt        # Python dependencies
├── Resume/                 # Directory for candidate resume PDFs
│   ├── resume_strong.pdf
│   ├── resume_average.pdf
│   └── resume_weak.pdf
└── README.md              # This file
```

## Configuration

### Job Description
Edit the job description in Cell 6 to customize requirements:
```python
job_description = """
Role: Data Scientist

Required Skills:
- Python
- Machine Learning
- NLP

Preferred Tools:
- TensorFlow
- PyTorch

Minimum Experience:
- 2+ years in relevant data/ML work
"""
```

### Model Selection
The pipeline uses Groq's `llama-3.3-70b-versatile` model. To change it, update Cell 4:
```python
llm = ChatGroq(
    model="your-model-name",
    temperature=0,  # 0 for deterministic output
)
```

### LangSmith Tracing
Tracing is enabled by default in Cell 2. Disable it by setting:
```python
os.environ["LANGCHAIN_TRACING_V2"] = "false"
```

## How It Works

### Pipeline Stages

1. **Extraction**: LLM extracts skills, tools, and experience from resume
2. **Matching**: Compares extracted data against job requirements
3. **Scoring**: Applies evaluation rules to generate score (0-100)
4. **Explanation**: Creates human-readable summary of decision

### Scoring Logic (Improved v2)

- Missing 2+ required skills → score ≤ 60
- All required skills + 2+ years experience → score ≥ 80
- Missing skills significantly reduce score
- No assumptions made about skills not explicitly stated

### Debugging Example

Cell 12 demonstrates:
- **Legacy Bug**: Over-weights years of experience, inflating scores for unqualified candidates
- **Fix**: Strict missing-skill penalties prevent false positives

## Troubleshooting

### API Key Errors
```
AuthenticationError: Invalid API key
```
**Solution**: Verify API keys are correctly set in Cell 2 and active in your Groq/LangSmith accounts.

### Model Deprecated
```
BadRequestError: The model has been decommissioned...
```
**Solution**: Update the model name in Cell 4 to a currently supported model from [Groq's model list](https://console.groq.com/docs/models).

### Missing Resume Files
```
Warning: Missing file for Strong: Resume/resume_strong.pdf
```
**Solution**: Ensure all PDF files exist in the `Resume/` directory with exact matching names.

### JSON Parsing Errors
Check that the LLM is returning valid JSON. Try:
- Rerunning the cell
- Adjusting the prompt instructions
- Using a different model

## Dependencies

See `requirements.txt` for full list. Key packages:
- `langchain-core`: LLM orchestration framework
- `langchain-community`: Community integrations (PDF loader)
- `langchain-groq`: Groq model integration
- `pypdf`: PDF parsing

## LangSmith Integration

All pipeline steps are automatically traced to LangSmith:
1. Access traces at: https://smith.langchain.com
2. Project name: `resume-screening-system`
3. Traces include tags: `extract`, `match`, `score`, `explain`

## Use Cases

- **Recruitment Automation**: Screen large volumes of resumes at scale
- **Bias Detection**: Compare scoring decisions across candidates
- **Academic Research**: Study LLM-based hiring decisions
- **Portfolio Project**: Demonstrate LangChain proficiency

## Best Practices

1. **Test with sample data** before using production resumes
2. **Review job description** for accuracy and completeness
3. **Monitor LangSmith traces** for quality assurance
4. **Store API keys securely** (use environment variables, not hardcoded)
5. **Validate outputs** before making hiring decisions

## Future Enhancements

- Batch processing with progress tracking
- Custom scoring rule templates
- Multi-language resume support
- Resume similarity clustering
- Feedback loop for score calibration

## License

This project is provided as-is for educational and commercial use.

## Author

Resume Screening System - Built with LangChain and Groq

---

**Last Updated**: April 2026

For questions or issues, review the notebook comments and LangSmith traces for debugging guidance.
