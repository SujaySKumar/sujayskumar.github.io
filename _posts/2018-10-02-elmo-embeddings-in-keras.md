---
layout: post
title: ELMo Embeddings in Keras
date: '2018-10-02T20:24:16.101262'
author: Sujay S Kumar
tags: 
---

In the previous blog post on [Transfer Learning](http://sujayskumar.com/2018/06/30/i-have-words-give-me-sentences-sentence/), we discovered how pre-trained models can be leveraged in our applications to save on train time, data, compute and other resources along with the added benefit of better performance. In this blog post, I will be demonstrating how to use ELMo Embeddings in Keras.

Pre-trained ELMo Embeddings are freely available as a [Tensorflow Hub Module](https://tfhub.dev/google/elmo/2). I prefer Keras for quick experimentation and iteration and hence I was looking at ways to use these models from the Hub directly in my Keras project. Unfortunately, this is not as straightforward as it initially seems to be. ELMo has 4 trainable parameters that needs to be trained/fine-tuned with your custom dataset. The expected behaviour in this scenario is that these weights get updated as part of the learning procedure of the entire network. On the contrary, these 4 learnable parameters refused to get updated and hence, I decided to write a custom layer in Keras that updates these weights manually.

Here is the code:
```python
elmo_model = hub.Module("https://tfhub.dev/google/elmo/2", trainable=True)
sess = tf.Session()

K.set_session(sess)
# Initialize sessions
sess.run(tf.global_variables_initializer())
sess.run(tf.tables_initializer())

class KerasLayer(Layer):

    def __init__(self, output_dim, **kwargs):
        self.output_dim = output_dim
        super(MyLayer, self).__init__(**kwargs)

    def build(self, input_shape):
        # Create a trainable weight variable for this layer.

        # These are the 3 trainable weights for word_embedding, lstm_output1 and lstm_output2
        self.kernel1 = self.add_weight(name='kernel1',
                                       shape=(3,),
                                      initializer='uniform',
                                      trainable=True)
        # This is the bias weight
        self.kernel2 = self.add_weight(name='kernel2',
                                       shape=(),
                                      initializer='uniform',
                                      trainable=True)
        super(MyLayer, self).build(input_shape)

    def call(self, x):
        # Get all the outputs of elmo_model
        model =  elmo_model(tf.squeeze(tf.cast(x, tf.string)), signature="default", as_dict=True)
        
        # Embedding activation output
        activation1 = model["word_emb"]
        
        # First LSTM layer output
        activation2 = model["lstm_outputs1"]
        
        # Second LSTM layer output
        activation3 = model["lstm_outputs2"]

        activation2 = tf.reduce_mean(activation2, axis=1)
        activation3 = tf.reduce_mean(activation3, axis=1)
        
        mul1 = tf.scalar_mul(self.kernel1[0], activation1)
        mul2 = tf.scalar_mul(self.kernel1[1], activation2)
        mul3 = tf.scalar_mul(self.kernel1[2], activation3)
        
        sum_vector = tf.add(mul2, mul3)
        
        return tf.scalar_mul(self.kernel2, sum_vector)

    def compute_output_shape(self, input_shape):
        return (input_shape[0], self.output_dim)
        
input_text = layers.Input(shape=(1,), dtype=tf.string)
custom_layer = KerasLayer(output_dim=1024, trainable=True)(input_text)
pred = layers.Dense(1, activation='sigmoid', trainable=False)(custom_layer)

model = Model(inputs=input_text, outputs=pred)

model.compile(loss='binary_crossentropy', optimizer='adam', metrics=['accuracy'])
print(model.summary())
model.fit(inp, target, epochs=15, batch_size=32)
```
