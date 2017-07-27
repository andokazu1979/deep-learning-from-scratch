%% Example of sequence diagram
sequenceDiagram
  Main->>MultiLayerNetExtend: MultiLayerNetExtend()
  MultiLayerNetExtend->>MultiLayerNetExtend: __init_weight(weight_init_std)
  MultiLayerNetExtend->>Affine: Affine()
  MultiLayerNetExtend-->>Main: network
  Main->>Trainer: Trainer(network)
  Trainer->>SGD: SGD()
  SGD-->>Trainer: optimizer
  Trainer-->>Main: trainer
  Main->>+Main: train()
  loop max_iter
    Main->>Trainer: train_step()
    Trainer->>+MultiLayerNetExtend: gradient()
    MultiLayerNetExtend->>+MultiLayerNetExtend: loss(x, t, train_flg=True)
    MultiLayerNetExtend->>+MultiLayerNetExtend: predict()
    MultiLayerNetExtend->>Affine: forward(x)
    MultiLayerNetExtend->>-MultiLayerNetExtend: predict()
    MultiLayerNetExtend->>-MultiLayerNetExtend: loss(x, t, train_flg=True)
    MultiLayerNetExtend->>Affine: backward(dout)
    MultiLayerNetExtend-->>-Trainer: grads
    Trainer->>SGD: update()
    Trainer->>MultiLayerNetExtend: loss()
    MultiLayerNetExtend-->>Trainer: loss
    opt iter_per_epoch
      Trainer->>MultiLayerNetExtend: accuracy()
      MultiLayerNetExtend-->>Trainer: acc
    end
  end
  Main->>-Main: train()
