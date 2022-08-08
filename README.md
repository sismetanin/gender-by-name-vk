# Detecting Gender of VKontakte Users by Full Names in Russian/English


## Data
We selected the top-1 VK group[^1] based on Medialogia rating[^2] and loaded all the users with specified gender via VKontakte API[^3]. We collected 13,126,794 Vkontake profiles, where each profile contains gender and first and last names written in Cyrillic and Latin alphabets. We did not consider users with unknown gender. 

Among these profiles, we got 6,521,854 unique full names in Cyrillic alphabet and 6,263,813 unique full names in Latin alphabet. The number of full names in the two alphabets differs due to the fact that different names in the Cyrillic alphabet can have both the same and different transliteration into the Latin alphabet, and even similar names in one alphabet can have different transliteration to another alphabet. Based on the data obtained, we formed the final dataset for training models using the following logic: if the user's name in Latin and Cyrillic is different, we added both names to the dataset, if they are the same, we added only one name. 

The final dataset contains 25,101,673 names (46\% male and 54\% female). 

## Model
Following the approach by Panchenko and Teterin[^4], we used L2-regularized Logistic Regression with character n-grams to classify gender. In order to identify the best hyper-parameters (e.g. character n-grams type, n-grams range, usage of IDF, TF-IDF normalisation type), we firstly ran a grid search with 10-fold cross-validation (80% – training subset, 20% – test subset) on a random sample of 100,000 full names. The model with character n-grams inside word boundaries, n-grams range of (2, 7), usage of IDF, L2 TF-IDF normalisation, and ignoring terms that appear in more than 50% of the documents showed the best $F_1$ score of 0.9771, so we used these hyper-parameters to train the final model on the whole dataset. The final model trained in the full dataset demonstrated $F_1=0.9835$ on the test subset (20% of full names).


[^1]: https://vk.com/public27895931
[^2]: https://www.mlg.ru/ratings/socmedia/vk/10828/
[^3]: https://dev.vk.com/method/groups.getMembers
[^4]: https://link.springer.com/chapter/10.1007/978-3-319-12580-0_17
