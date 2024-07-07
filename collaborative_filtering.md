# Collaborative Filtering Overview

The core idea behind collaborative filtering is that people often receive the best recommendations from others with similar tastes. By identifying users with similar interests, collaborative filtering algorithms can provide personalized recommendations.

## Workflow

1. **Rating Items**: Users rate items (e.g., books, movies, or music), providing a representation of their interests.
2. **Matching Users**: The system compares these ratings with those of other users to find individuals with similar tastes.
3. **Making Recommendations**: Based on these similarities, the system recommends items that like-minded users have rated highly but the user has not yet rated.

![Collaborative Filtering](https://upload.wikimedia.org/wikipedia/commons/thumb/5/52/Collaborative_filtering.gif/450px-Collaborative_filtering.gif)

## Code for Collaborative Filtering

```python
import pandas as pd
import numpy as np

# Example user-item interaction data
data = {
    'user_id': [1, 1, 1, 2, 2, 3, 3, 4, 4],
    'item_id': [10, 20, 30, 10, 40, 20, 30, 10, 40],
    'rating': [4, 5, 3, 5, 4, 2, 5, 5, 3]
}


df = pd.DataFrame(data)


user_item_matrix = df.pivot_table(index='user_id', columns='item_id', values='rating')


user_item_matrix = user_item_matrix.fillna(0)

from sklearn.metrics.pairwise import cosine_similarity

user_similarity = cosine_similarity(user_item_matrix)
user_similarity_df = pd.DataFrame(user_similarity, index=user_item_matrix.index, columns=user_item_matrix.index)


def predict_ratings(user_item_matrix, user_similarity):
    mean_user_rating = user_item_matrix.mean(axis=1)
    ratings_diff = (user_item_matrix.T - mean_user_rating).T
    pred = mean_user_rating[:, np.newaxis] + user_similarity.dot(ratings_diff) / np.array([np.abs(user_similarity).sum(axis=1)]).T
    return pred


predicted_ratings = predict_ratings(user_item_matrix.values, user_similarity)


predicted_ratings_df = pd.DataFrame(predicted_ratings, index=user_item_matrix.index, columns=user_item_matrix.columns)


def get_recommendations(user_id, user_item_matrix, predicted_ratings_df, num_recommendations=5):
    user_idx = user_item_matrix.index.get_loc(user_id)
    user_ratings = user_item_matrix.iloc[user_idx]
    sorted_ratings = predicted_ratings_df.iloc[user_idx].sort_values(ascending=False)
    recommendations = sorted_ratings[~user_ratings.index.isin(user_ratings[user_ratings > 0].index)]
    return recommendations.head(num_recommendations)


user_id = 1
recommendations = get_recommendations(user_id, user_item_matrix, predicted_ratings_df)
print(f"Top recommendations for user {user_id}:")
print(recommendations)

```

