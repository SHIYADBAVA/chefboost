# chefboost

<p align="center"><img src="https://raw.githubusercontent.com/serengil/chefboost/master/icon/chefboost.jpg" width="200" height="200"></p>

Chefboost is [gradient boosting](https://sefiks.com/2018/10/04/a-step-by-step-gradient-boosting-decision-tree-example/), [random forest](https://sefiks.com/2017/11/19/how-random-forests-can-keep-you-from-decision-tree/) and [adaboost](https://sefiks.com/2018/11/02/a-step-by-step-adaboost-example/) enabled decision tree framework including regular [ID3](https://sefiks.com/2017/11/20/a-step-by-step-id3-decision-tree-example/), [C4.5](https://sefiks.com/2018/05/13/a-step-by-step-c4-5-decision-tree-example/), [CART](https://sefiks.com/2018/08/27/a-step-by-step-cart-decision-tree-example/) and [regression tree](https://sefiks.com/2018/08/28/a-step-by-step-regression-decision-tree-example/) algorithms **with categorical features support**.

# Usage

Basically, you just need to pass the dataset as pandas data frame and tree configurations after importing Chefboost as illustrated below. You just need to put the target label to the right. Besides, chefboost handles both numeric and nominal features and target values in contrast to its alternatives.

```
import Chefboost as chef
import pandas as pd

config = {
	'algorithm': 'ID3' #ID3, C4.5, CART, Regression
	, 'enableGBM': False, 'epochs': 10, 'learning_rate': 1
	, 'enableRandomForest': False, 'num_of_trees': 5, 'enableMultitasking': False
	, 'enableAdaboost': False, 'num_of_weak_classifier': 4
	, 'debug': False
}

df = pd.read_csv("dataset/golf.txt")

chef.fit(df, config)
```

# Outcomes

Built decision trees are stored as python if statements in the `outputs/rules/rules.py` file. A sample of decision rules is demonstrated below.

```
def findDecision(Outlook,Temperature,Humidity,Wind,Decision):
   if Outlook == 'Rain':
      if Wind == 'Weak':
         return 'Yes'
      elif Wind == 'Strong':
         return 'No'
   elif Outlook == 'Sunny':
      if Humidity == 'High':
         return 'No'
      elif Humidity == 'Normal':
         return 'Yes'
   elif Outlook == 'Overcast':
      return 'Yes'
 ```

# Testing for custom instances

Decision rules will be stored in `outputs/rules/` folder when you build a decision tree. You can run the built decision tree for new instances as illustrated below.

```
import imp

moduleName = "outputs/rules/rules" #this will call rules.py
fp, pathname, description = imp.find_module(moduleName)
myrules = imp.load_module(moduleName, fp, pathname, description)

prediction = myrules.findDecision(['Overcast', 'Hot', 'High', 'Weak'])

print(prediction)
```

Recursive algorithms such as GBM creates multiple rules in that directory. You need to specify the round index for this case.

```
epochs = config['num_of_trees']
prediction = 0
for i in range(0, epochs):
   moduleName = "outputs/rules/rules%s" % (i)
   fp, pathname, description = imp.find_module(moduleName)
   myrules = imp.load_module(moduleName, fp, pathname, description)
   
   prediction += myrules.findDecision(['Overcast', 'Hot', 'High', 'Weak'])

print("Boosted prediction: ",prediction)
```

In Adaboost, you also need to round predictions and round alpha values



# Prerequisites

Pandas and numpy python libraries are used to load data sets in this repository. You might run the following commands to install these packages if you are going to use them first time.

```
pip install pandas==0.22.0
pip install numpy==1.14.0
pip install tqdm==4.30.0
```

Initial tests are run on the following environment.

 ```
C:\>python --version
Python 3.6.4 :: Anaconda, Inc.
 ```
 
# Documentation

You can find detailed documentations about these core algorithms [here](https://sefiks.com/tag/decision-tree/). Besides, this YouTube [playlist](https://www.youtube.com/playlist?list=PLsS_1RYmYQQHp_xZObt76dpacY543GrJD) guides you how to use Chefboost step by step.

# Licence

Chefboost is licensed under the MIT License - see [`LICENSE`](https://github.com/serengil/chefboost/blob/master/LICENSE) for more details.
