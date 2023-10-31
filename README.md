# gradient_descent_learning

This is the code for analysing the data from a rerun of https://osf.io/preprints/psyarxiv/75e4t/ .

The code contains:
- Initial data wrangling
- A maximum likelihood model that outputs the strategy a participant was using for a trial
- A Hidden Markov Model that infers hidden states based on the output of the maximum likelihood model

## Data details `/data` 

This directory contains the initial data and preprocessing pipeline.

`datawrangle.py`: Collects all participant responses into a single csv file

`fullcode.csv`: Code file for decoding randomisation

`cleaned_pilotdata.csv`: output from `datawrangle.py`. Contains participant trials along with randomisation coding.

`/pilotdata`: contains initial raw data from 10 participants.

---

## Likelihood Model `/likelihood_model`

`model_fitting.py`

This model finds the optimal strategy for a given participant trial.

Each participant has four observations per trial (e.g., [21211, 21121, 21221, 11211]).

The script first decodes the strong and weak features and then based on the trial observations finds the strategy that is most likely to result in the given observations.

The model **outputs** a csv file `/likel_results/model_fit_results.csv`:

This contains the likelihoods and guessing parameter (alpha) values for each strategy. 

It also contains three columns with alternative ways of deriving the *best_strategy*:

- *best_strategy_guess*: If multiple strategies have the same likelihood, the label "guessing" is set as the value for that trial
- *best_strategy_rand*: If multiple strategies have the same likelihood, a strategy is selected at random from the equally likely strategies
- *best_strategies*: If multiple strategies have the same likelihood, a list containing all the equally likely strategies is set as the value for that trial
---

## Hidden Markov Model `/hmm`

### Data preprocessing

`hmm_data.py`: Reads in `model_fit_results.csv` and generates three .csv files with the following templates:

under `/hmm_data`:




**FOR:** 
`/hmm_data_best_strategy_rand.csv` & `/hmm_data_best_strategies.csv` 

| Strong | Weak1 | Weak 2 | Prototype |
| ----------- | ----------- | ----------- | ----------- |
| 0/1 | 0/1 | 0/1 | 0/1 |

**FOR:**
`/hmm_data_best_strategy_guess.csv`
| Strong | Weak1 | Weak 2 | Prototype | Guessing |
| ----------- | ----------- | ----------- | ----------- | ----------- |
| 0/1 | 0/1 | 0/1 | 0/1 | 0/1 |


### HMM search
`hmm_search.py`: Searches for the optimal number of hidden states based on AIC and BIC. Shows a plot with AIC and BIC values for each number of hidden states.

### HMM fit

`hmm_test.py`: Fits 1000 randomly initialised HMMs with a set number of hidden states.

**Outputs**: 
- The best model (`/hmm_results/best_model_states_X.pkl`)
- Inferred states for each trial (`/hmm_results/hmm_results_states_X.csv`).

---

## Plots `/plots`

`alpha_plot.py`: Plots the average guessing parameter (alpha) across trials for all strategies

`strategy_plot.py`: Plots the average use of strategies (likelihood model) across trials

`hmm_plot.py`: Plots the HMM model parameters as a graph

---

## Analysis Procedure

**TODO**


