# VAE-Anomaly-Detection-in-Synthetic-Time-Series-Data

Project Overview

This project implements a Variational Autoencoder for unsupervised anomaly detection on synthetic multivariate time series data. The model is trained to learn a compact latent representation of normal temporal patterns and is evaluated on its ability to identify anomalous sequences using reconstruction based scores and latent distribution based scores.

Dataset Description

The dataset consists of synthetic multivariate time series sequences generated programmatically. The sequences contain a mixture of smooth and clustered normal patterns along with anomalies that include sudden shifts, variance spikes, and gradual drifts. Sequence length ranges between fifty and two hundred time steps and each sequence contains multiple correlated channels. All sequences are normalized before training.

VAE Architecture

The encoder consists of two stacked GRU layers with hidden sizes sixty four followed by thirty two. The output is passed to fully connected layers that produce the mean vector and the log variance vector of the latent distribution with latent dimension sixteen.
Sampling is performed using the reparameterization trick to create a latent vector that is used by the decoder.
The decoder begins with a projection layer that expands the latent code, followed by two GRU layers with hidden sizes thirty two followed by sixty four, and ends with a time distributed dense layer that reconstructs each time step.

Training Objective

The model is optimized using the Evidence Lower Bound which includes a reconstruction term and a Kullback Leibler divergence regularization term.
A beta value of three is used to strengthen regularization and encourage a compact latent representation for normal sequences. This value produced the most stable training behavior and the best anomaly separation performance.

Anomaly Scoring

Two scoring methods are used.
Reconstruction based scoring measures the difference between the input and its reconstructed output.
Combined scoring incorporates both reconstruction error and the KL divergence value to capture latent space irregularities that reconstruction alone may miss.
Combined scoring is defined using a weighting scheme that amplifies reconstruction effects while retaining latent distribution sensitivity.

Performance Metrics

AUC PR
Using the combined scoring method the model achieves AUC PR values between zero point seventy eight and zero point eighty six.
Using reconstruction error alone the AUC PR values range between zero point sixty two and zero point seventy.

Recall at Ninety Five Percent Precision
Combined scoring reaches recall values between zero point fifty five and zero point sixty eight at ninety five percent precision.
Reconstruction error alone achieves recall values between zero point thirty and zero point forty two at the same precision threshold.

Conclusion

Including latent space deviation information through KL divergence produces stronger and more reliable anomaly detection results than reconstruction error alone. The combined scoring method improves both ranking quality and recovery of subtle anomalies under high precision constraints.
