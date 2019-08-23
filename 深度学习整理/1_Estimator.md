## Transformer+DNN

- Transformer.py：完成Transformer模型构建
- Evaluator.py：该类集成了训练参数，和各子模型。搭建transformer+DNN
- main.py：训练入口


```python
def model_fn(features, labels, mode):
    params = {
        "batch_size": FLAGS.batch_size,
        "feature_dim": FLAGS.feature_dim,
        "length": FLAGS.length,
        "l2_reg": FLAGS.l2_reg,
        "learning_rate": FLAGS.learning_rate,
        "latent_layer": FLAGS.latent_layer,
        "cell_dim": FLAGS.cell_dim,
        "optimizer": FLAGS.optimizer,
        "dropout_keep_prob_p": FLAGS.dropout_keep_prob_p,
        "sequence_encoder": FLAGS.sequence_encoder
    }
    model = Evaluator(features,
                      labels,
                      mode,
                      params)
    return model.build_graph()
def main():
    run_config = tf.estimator.RunConfig(save_summary_steps=FLAGS.batch_size * 10,keep_checkpoint_max=3)
    classifier = tf.estimator.Estimator(
        model_fn=model_fn,
        model_dir=FLAGS.checkpoint_path,
        config=run_config
    )

    if FLAGS.mode == "train_and_eval":
        train_spec = tf.estimator.TrainSpec(
            input_fn=lambda: _input_fn(FLAGS.train_path, FLAGS.batch_size, epochs=FLAGS.epochs, prefetch=True))

        eval_spec = tf.estimator.EvalSpec(
            input_fn=lambda: _input_fn(FLAGS.test_path, FLAGS.batch_size, epochs=FLAGS.epochs, prefetch=False),
            throttle_secs=60
        )
        train, val = tf.estimator.train_and_evaluate(
            classifier,
            train_spec,
            eval_spec)
       
    elif FLAGS.mode == "eval":
        print("checkpoint_path {}".format(FLAGS.checkpoint_path))
        evaluate_result = classifier.evaluate(
            input_fn=lambda: _input_fn(FLAGS.test_path, FLAGS.batch_size, epochs=FLAGS.epochs, prefetch=False),
            steps=2,
            checkpoint_path=FLAGS.checkpoint_path,
        )
        for key in evaluate_result:
            print("   {}, was: {}".format(key, evaluate_result[key]))

```








