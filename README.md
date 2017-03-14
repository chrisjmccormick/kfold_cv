I've written some functions which can help you divide your data set into training and validation sets for n-fold cross-validation.

You should have all of your data points in a matrix X where each row is a separate data point. You should also have a column vector y containing the category (or class label) for each of the corresponding data points.

You will also need to define a column vector 'categories' which just lists the class label values you are using. I require this so that the code doesn't make any assumptions about the values you are using for your class labels. You could use '0' as one of the categories if you want, for example, and the values don't have to be contiguous.

Here are links to each of the functions, with a short description of what each does. There is also a simple example usage at the end.

[getVecsPerCat.m](https://www.dropbox.com/s/c97ibrlum5qom75/getVecsPerCat.m?dl=0) - Counts the number of vectors belonging to each category.

[computeFoldSizes.m](https://www.dropbox.com/s/5c9aeufxildijcx/computeFoldSizes.m?dl=0) - Pre-compute the size of each of the n folds for each category. The number of folds might not divide evenly into the number of vectors, so this function handles distributing the remainder across the folds.

[randSortAndGroup.m](https://www.dropbox.com/s/wdavzcosu3k7s51/randSortAndGroup.m?dl=0) - Sorts the vectors by category, and randomizes the order of the vectors within each category.

[getFoldVectors.m](https://www.dropbox.com/s/hocy73dky01mjg5/getFoldVectors.m?dl=0) - For the specified round of cross-validation, selects X_train, y_train (the vectors to use for training, with their associated categories) and X_val, y_val (the vectors to use for validation, with their associated categories).

After calling getFoldVectors, it's up to you to perform the actual training, and compute your validation accuracy on the validation vectors. Below is some sample code for using the above functions, but note that it ommits the actual training and validation steps.

```Matlab
% List out the category values in use.
categories = [0; 1];

% Get the number of vectors belonging to each category.
vecsPerCat = getVecsPerCat(X, y, categories);

% Compute the fold sizes for each category.
foldSizes = computeFoldSizes(vecsPerCat, 10);

% Randomly sort the vectors in X, then organize them by category.
[X_sorted, y_sorted] = randSortAndGroup(X, y, categories);

% For each round of cross-validation...
for (roundNumber = 1 : 10)

% Select the vectors to use for training and cross validation.
[X_train, y_train, X_val, y_val] = getFoldVectors(X_sorted, y_sorted, categories, vecsPerCat, foldSizes, roundNumber);

% Train the classifier on the training set, X_train y_train
% .....................

% Measure the classification accuracy on the validation set.
% .....................

end
```