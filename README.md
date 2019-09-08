# PyPMML

_PyPMML_ is a Python PMML scoring library, it really is the Python API for [PMML4S](https://github.com/autodeployai/pmml4s).

## Prerequisites
 - Java >= 1.8
 - Python 2.7 or >= 3.5

## Dependencies
  - Py4J
  - Pandas (optional)
  
## Installation

```bash
pip install pypmml
```

Or install the latest version from github:

```bash
pip install --upgrade git+https://github.com/autodeployai/pypmml.git
```

## Usage
1. Load model from various sources, e.g. filename, string, or array of bytes.

    ```python
    from pypmml import Model
    
    # The model is from http://dmg.org/pmml/pmml_examples/KNIME_PMML_4.1_Examples/single_iris_dectree.xml
    model = Model.fromFile('single_iris_dectree.xml')
    ```

2. Call `predict(data)` to predict new values that can be in different types, e.g. dict, json, Series or DataFrame of Pandas.

    ```python
    # data in dict
    result = model.predict({'sepal_length': 5.1, 'sepal_width': 3.5, 'petal_length': 1.4, 'petal_width': 0.2})
     >>> print(result)
    {'probability': 1.0, 'node_id': '1', 'probability_Iris-virginica': 0.0, 'probability_Iris-setosa': 1.0, 'probability_Iris-versicolor': 0.0, 'predicted_class': 'Iris-setosa'}
    
    # data in 'records' json
    result = model.predict('[{"sepal_length": 5.1, "sepal_width": 3.5, "petal_length": 1.4, "petal_width": 0.2}]')
     >>> print(result)
    [{"probability":1.0,"probability_Iris-versicolor":0.0,"probability_Iris-setosa":1.0,"probability_Iris-virginica":0.0,"predicted_class":"Iris-setosa","node_id":"1"}]
 
    # data in 'split' json
    result = model.predict('{"columns": ["sepal_length", "sepal_width", "petal_length", "petal_width"], "data": [[5.1, 3.5, 1.4, 0.2]]}')
     >>> print(result)
    {"columns":["predicted_class","probability","probability_Iris-setosa","probability_Iris-versicolor","probability_Iris-virginica","node_id"],"data":[["Iris-setosa",1.0,1.0,0.0,0.0,"1"]]}
    ```

    How to work with Pandas
    
    ```python
    import pandas as pd
    
    # data in Series
    result = model.predict(pd.Series({'sepal_length': 5.1, 'sepal_width': 3.5, 'petal_length': 1.4, 'petal_width': 0.2}))
    >>> print(result)
    node_id                                   1
    predicted_class                 Iris-setosa
    probability                               1
    probability_Iris-setosa                   1
    probability_Iris-versicolor               0
    probability_Iris-virginica                0
    Name: 0, dtype: object
    
    # The data is from here: http://dmg.org/pmml/pmml_examples/Iris.csv
    data = pd.read_csv('Iris.csv')
    
    # data in DataFrame
    result = model.predict(data)
     >>> print(result)
        node_id   predicted_class  probability  probability_Iris-setosa  probability_Iris-versicolor  probability_Iris-virginica
    0         1       Iris-setosa     1.000000                      1.0                     0.000000                    0.000000
    1         1       Iris-setosa     1.000000                      1.0                     0.000000                    0.000000
    ..      ...               ...          ...                      ...                          ...                         ...
    148      10    Iris-virginica     0.978261                      0.0                     0.021739                    0.978261
    149      10    Iris-virginica     0.978261                      0.0                     0.021739                    0.978261
    
    [150 rows x 6 columns]
    ```

## Use PMML in Scala or Java
See the [PMML4S](https://github.com/autodeployai/pmml4s) project. _PMML4S_ a PMML scoring library for Scala. It provides both Scala and Java Evaluator API for PMML.

## Use PMML in Spark
See the [PMML4S-Spark](https://github.com/autodeployai/pmml4s-spark) project. _PMML4S-Spark_ is a PMML scoring library for Spark as SparkML Transformer.

## Use PMML in PySpark
See the [PyPMML-Spark](https://github.com/autodeployai/pypmml-spark) project. _PyPMML-Spark_ is a Python PMML scoring library for PySpark as SparkML Transformer, it really is the Python API for PMML4s-Spark.

## Deploy PMML as REST API
See the [DaaS](https://www.autodeploy.ai/) system that deploys AI & ML models in production at scale on Kubernetes.

## Support
If you have any questions about the _PyPMML_ library, please open issues on this repository.

Feedback and contributions to the project, no matter what kind, are always very welcome. 

## License
_PyPMML_ is licensed under [APL 2.0](http://www.apache.org/licenses/LICENSE-2.0).
