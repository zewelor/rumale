# 0.12.9
- Add class for K-Medoids clustering.
- Fix extension codes of decision tree regressor for using Numo::NArray.

# 0.12.8
- Fix bug that fails to build and install on Windows again. Fix extconf to add Numo::NArray libraries to $lib.

# 0.12.7
- Fix bug that fails to build and install on Windows. Add search for Numo::NArray static library path to extconf.

# 0.12.6
- Fix extension codes of decision tree classifier and gradient tree regressor for using Numo::NArray.

# 0.12.5
- Fix random number generator initialization on gradient boosting estimators
to obtain the same result with and without parallel option.

# 0.12.4
- Add class for multidimensional scaling.
- Fix parameter description on artificial dataset generation method.

# 0.12.3
- Add class for Power Iteration clustering.
- Add classes for artificial dataset generation.

# 0.12.2
- Add class for cluster analysis with Gaussian Mixture Model.
- Add encoder class for categorical features.

# 0.12.1
- Refactor kernel support vector classifier.
- Refactor random sampling on tree estimators.

# 0.12.0
## Breaking changes
- For reproductivity, Rumale changes to not repeatedly use the same random number generator in the same estimator.
In the training phase, estimators use a copy of the random number generator created in the initialize method.
Even with the same algorithm and the same data, the order of random number generation
may make slight differences in learning results.
By this change, even if the fit method is executed multiple times,
the same learning result can be obtained if the same data is given.

```ruby
svc = Rumale::LinearModel::SVC.new(random_seed: 0)
svc.fit(x, y)
a = svc.weight_vec
svc.fit(x, y)
b = svc.weight_vec
err = ((a - b)**2).mean

# In version 0.11.0 or earlier, false may be output,
# but from this version, true is always output.
puts(err < 1e-4)
```

# 0.11.0
- Introduce [Parallel gem](https://github.com/grosser/parallel) to improve execution speed for one-vs-the-rest and bagging methods.
- Add the n_jobs parameter that specifies the number of jobs for parallel processing in some estimators belong to the Rumale::LinearModel, Rumale::PolynomialModel, and Rumale::Ensemble.
- The n_jobs parameter is valid only when parallel gem is loaded.

```ruby
require 'rumale'
require 'parallel'

svc = Rumale::LinearModel::SVC.new(n_jobs: -1)
```

# 0.10.0
- Add class for t-distributed Stochastic Neighborhood Embedding.
- Fix bug of zero division on min-max scaling class.

# 0.9.2
- Add class for Gradient tree boosting classifier.
- Add class for Gradient tree boosting regressor.
- Add class for discretizing feature values.
- Refactor extra-trees estimators.
- Refactor decision tree base class.
- Fix some typos on document ([#6](https://github.com/yoshoku/rumale/pull/6)).

# 0.9.1
- Add class for Extra-Trees classifier.
- Add class for Extra-Trees regressor.
- Refactor extension modules of decision tree estimators for improving performance.

# 0.9.0
## Breaking changes
- Decide to introduce Ruby extensions for improving performance.
- Fix to find split point on decision tree estimators using extension modules.

# 0.8.4
- Remove unused parameter on Nadam.
- Fix condition to stop growing tree about decision tree.

# 0.8.3
- Add optimizer class for AdaGrad.
- Add evaluator class for ROC AUC.
- Add class for scaling with maximum absolute value.

# 0.8.2
- Add class for Adam optimizer.
- Add data splitter classes for random permutation cross validation.
- Add accessor method for number of splits to K-fold splitter classes.
- Add execution result of example script on README ([#3](https://github.com/yoshoku/rumale/pull/3)).

# 0.8.1
- Add some evaluator classes.
  - MeanSquaredLogError
  - MedianAbsoluteError
  - ExplainedVarianceScore
  - AdjustedRandScore
  - MutualInformation
- Refactor normalized mutual infomation evaluator.
- Fix typo on document ([#2](https://github.com/yoshoku/rumale/pull/2)).

# 0.8.0
## Breaking changes
- Rename SVMKit to Rumale.
- Rename SGDLienareEstimator class to BaseLienarModel class.
- Add data type option to load_libsvm_file method. By default, the method represents the feature with Numo::DFloat.

## Refactoring
- Refactor factorization machine estimators.
- Refactor decision tree estimators.

# 0.7.3
- Add class for grid search performing hyperparameter optimization.
- Add argument validations to Pipeline.

# 0.7.2
- Add class for Pipeline that constructs chain of transformers and estimators.
- Fix some typos on document ([#1](https://github.com/yoshoku/SVMKit/pull/1)).

# 0.7.1
- Fix to use CSV class in parsing libsvm format file.
- Refactor ensemble estimators.

# 0.7.0
- Add class for AdaBoost classifier.
- Add class for AdaBoost regressor.

# 0.6.3
- Fix bug on setting random seed and max_features parameter of Random Forest estimators.

# 0.6.2
- Refactor decision tree classes for improving performance.

# 0.6.1
- Add abstract class for linear estimator with stochastic gradient descent.
- Refactor linear estimators to use linear esitmator abstract class.
- Refactor decision tree classes to avoid unneeded type conversion.

# 0.6.0
- Add class for Principal Component Analysis.
- Add class for Non-negative Matrix Factorization.

# 0.5.2
- Add class for DBSCAN clustering.

# 0.5.1
- Fix bug on class probability calculation of DecisionTreeClassifier.

# 0.5.0
- Add class for K-Means clustering.
- Add class for evaluating purity.
- Add class for evaluating normalized mutual information.

# 0.4.1
- Add class for linear regressor.
- Add class for SGD optimizer.
- Add class for RMSProp optimizer.
- Add class for YellowFin optimizer.
- Fix to be able to select optimizer on estimators of LineaModel and PolynomialModel.

# 0.4.0
## Breaking changes

SVMKit introduces optimizer algorithm that calculates learning rates adaptively
on each iteration of stochastic gradient descent (SGD).
While Pegasos SGD runs fast, it sometimes fails to optimize complicated models
like Factorization Machine.
To solve this problem, in version 0.3.3, SVMKit introduced optimization with RMSProp on
FactorizationMachineRegressor, Ridge and Lasso.
This attempt realized stable optimization of those estimators.
Following the success of the attempt, author decided to use modern optimizer algorithms
with all SGD optimizations in SVMKit.
Through some preliminary experiments, author implemented Nadam as the default optimizer.
SVMKit plans to add other optimizer algorithms sequentially, so that users can select them.

- Fix to use Nadam for optimization on SVC, SVR, LogisticRegression, Ridge, Lasso, and Factorization Machine estimators.
  - Combine reg_param_weight and reg_param_bias parameters on Factorization Machine estimators into the unified parameter named reg_param_linear.
  - Remove init_std paramter on Factorization Machine estimators.
  - Remove learning_rate, decay, and momentum parameters on Ridge, Lasso, and FactorizationMachineRegressor.
  - Remove normalize parameter on SVC, SVR, and LogisticRegression.

# 0.3.3
- Add class for Ridge regressor.
- Add class for Lasso regressor.
- Fix bug on gradient calculation of FactorizationMachineRegressor.
- Fix some documents.

# 0.3.2
- Add class for Factorization Machine regressor.
- Add class for Decision Tree regressor.
- Add class for Random Forest regressor.
- Fix to support loading and dumping libsvm file with multi-target variables.
- Fix to require DecisionTreeClassifier on RandomForestClassifier.
- Fix some mistakes on document.

# 0.3.1
- Fix bug on decision function calculation of FactorizationMachineClassifier.
- Fix bug on weight updating process of KernelSVC.

# 0.3.0
- Add class for Support Vector Regression.
- Add class for K-Nearest Neighbor Regression.
- Add class for evaluating coefficient of determination.
- Add class for evaluating mean squared error.
- Add class for evaluating mean absolute error.
- Fix to use min method instead of sort and first methods.
- Fix cross validation class to be able to use for regression problem.
- Fix some typos on document.
- Rename spec filename for Factorization Machine classifier.

# 0.2.9
- Add predict_proba method to SVC and KernelSVC.
- Add class for evaluating logarithmic loss.
- Add classes for Label- and One-Hot- encoding.
- Add some validator.
- Fix bug on training data score calculation of cross validation.
- Fix fit method of SVC for performance.
- Fix criterion calculation on Decision Tree for performance.
- Fix data structure of Decision Tree for performance.

# 0.2.8
- Fix bug on gradient calculation of Logistic Regression.
- Fix to change accessor of params of estimators to read only.
- Add parameter validation.

# 0.2.7
- Fix to support multiclass classifiction into LinearSVC, LogisticRegression, KernelSVC, and FactorizationMachineClassifier

# 0.2.6
- Add class for Decision Tree classifier.
- Add class for Random Forest classifier.
- Fix to use frozen string literal.
- Refactor marshal dump method on some classes.
- Introduce Coveralls to confirm test coverage.

# 0.2.5
- Add classes for Naive Bayes classifier.
- Fix decision function method on Logistic Regression class.
- Fix method visibility on RBF kernel approximation class.

# 0.2.4
- Add class for Factorization Machine classifier.
- Add classes for evaluation measures.
- Fix the method for prediction of class probability in Logistic Regression.

# 0.2.3
- Add class for cross validation.
- Add specs for base modules.
- Fix validation of the number of splits when a negative label is given.

# 0.2.2
- Add data splitter classes for K-fold cross validation.

# 0.2.1
- Add class for K-nearest neighbors classifier.

# 0.2.0
- Migrated the linear algebra library to Numo::NArray.
- Add module for loading and saving libsvm format file.

# 0.1.3
- Add class for Kernel Support Vector Machine with Pegasos algorithm.
- Add module for calculating pairwise kernel fuctions and euclidean distances.

# 0.1.2
- Add the function learning a model with bias term to the PegasosSVC and LogisticRegression classes.
- Rewrite the document with yard notation.

# 0.1.1
- Add class for Logistic Regression with SGD optimization.
- Fix some mistakes on the document.

# 0.1.0
- Add basic classes.
- Add an utility module.
- Add class for RBF kernel approximation.
- Add class for Support Vector Machine with Pegasos alogrithm.
- Add class that performs mutlclass classification with one-vs.-rest strategy.
- Add classes for preprocessing such as min-max scaling, standardization, and L2 normalization.
