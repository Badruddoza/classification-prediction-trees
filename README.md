# from decision trees to random forests
# step-by-step guide

library(rpart)
x <- cbind(x_train,y_train)
# grow tree 
fit <- rpart(y_train ~ ., data = x,method="class")
summary(fit)
#Predict Output 
predicted= predict(fit,x_test)

library(randomForest)
x <- cbind(x_train,y_train)
# Fitting model
fit <- randomForest(Species ~ ., x,ntree=500)
summary(fit)
#Predict Output 
predicted= predict(fit,x_test)

# boosting
library(caret)
fitControl <- trainControl(method = "cv", number = 10, #5folds)
tune_Grid <-  expand.grid(interaction.depth = 2,
                            n.trees = 500,
                            shrinkage = 0.1,
                            n.minobsinnode = 10)
set.seed(123)
fit <- train(y_train ~ ., data = train,
                 method = "gbm",
                 trControl = fitControl,
                 verbose = FALSE,
                 tuneGrid = gbmGrid)
predicted= predict(fit,test,type= "prob")[,2]
