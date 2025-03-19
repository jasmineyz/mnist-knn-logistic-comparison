library(tidyverse)
library(caret)
library(dslabs)
library(gridExtra)

data("mnist_27")

# --------------------
# Visualizing Test Dataset
# --------------------
mnist_27$test %>%
  ggplot(aes(x_1, x_2, color = y)) +
  geom_point()

# --------------------
# Train and Evaluate kNN Model (k = 5)
# --------------------
knn_fit <- knn3(y ~ ., data = mnist_27$train)

y_hat_knn <- predict(knn_fit, mnist_27$test, type = "class")
kNN_5_accuracy <- confusionMatrix(y_hat_knn, mnist_27$test$y)$overall["Accuracy"]

# --------------------
# Train and Evaluate Logistic Regression Model
# --------------------
# Observation: Logistic regression provides linear decision boundaries.
fit_lm <- mnist_27$train %>% 
  mutate(y = ifelse(y == 7, 1, 0)) %>%
  lm(y ~ x_1 + x_2, data = .)

p_hat_lm <- predict(fit_lm, mnist_27$test)
y_hat_lm <- factor(ifelse(p_hat_lm > 0.5, 7, 2))
logistic_accuracy <- confusionMatrix(y_hat_lm, mnist_27$test$y)$overall["Accuracy"]

# --------------------
# Visualizing Conditional Probabilities
# --------------------
plot_cond_prob <- function(p_hat=NULL){
  tmp <- mnist_27$true_p
  if(!is.null(p_hat)){
    tmp <- mutate(tmp, p=p_hat)
  }
  tmp %>% ggplot(aes(x_1, x_2, z=p, fill=p)) +
    geom_raster(show.legend = FALSE) +
    scale_fill_gradientn(colors=c("#F8766D", "white", "#00BFC4")) +
    stat_contour(breaks=c(0.5), color="black")
}

p1 <- plot_cond_prob() + ggtitle("True conditional probability")
p2 <- plot_cond_prob(predict(knn_fit, mnist_27$true_p)[,2]) + ggtitle("kNN-5 estimate")
grid.arrange(p2, p1, nrow=1)

# --------------------
# Overtraining (k = 1)
# --------------------
# Observation: kNN with k=1 results in high variance and overfitting to noise.
knn_fit_1 <- knn3(y ~ ., data = mnist_27$train, k = 1)
y_hat_knn_1 <- predict(knn_fit_1, mnist_27$test, type = "class")
kNN_1_accuracy <- confusionMatrix(y_hat_knn_1, mnist_27$test$y)$overall["Accuracy"]

# --------------------
# Oversmoothing (k = 401)
# --------------------
# Observation: kNN with k=401 leads to high bias and overly smooth decision boundaries.
knn_fit_401 <- knn3(y ~ ., data = mnist_27$train, k = 401)
y_hat_knn_401 <- predict(knn_fit_401, mnist_27$test, type = "class")
kNN_401_accuracy <- confusionMatrix(y_hat_knn_401, mnist_27$test$y)$overall["Accuracy"]

# --------------------
# Compare Logistic Regression vs. kNN-401
# --------------------
fit_glm <- glm(y ~ x_1 + x_2, data=mnist_27$train, family="binomial")

p1 <- plot_cond_prob(predict(fit_glm, mnist_27$true_p)) + ggtitle("Regression")
p2 <- plot_cond_prob(predict(knn_fit_401, mnist_27$true_p)[,2]) + ggtitle("kNN-401")
grid.arrange(p1, p2, nrow=1)
