# 在Pytorch中，类rnn结构处理变长序列的方式
问题来源：在我训练[dien](http://www.baidu.com)网络的时候，不同样本的历史行为数据的长度不一
在批量化训练类rnn结构时，如何更优雅的实现？

## 1.  从rnn源码说起
为了方便叙述，pytorch源码中,rnn、lstm、gru的处理方式类似，这里就以[lstm](https://github.com/pytorch/pytorch/blob/master/torch/nn/modules/rnn.py)为例
``` python
    def forward(self, input, hx=None):  # noqa: F811
        orig_input = input
        # xxx: isinstance check needs to be in conditional for TorchScript to compile
        if isinstance(orig_input, PackedSequence):
            input, batch_sizes, sorted_indices, unsorted_indices = input
            max_batch_size = batch_sizes[0]
            max_batch_size = int(max_batch_size)
        else:
            batch_sizes = None
            max_batch_size = input.size(0) if self.batch_first else input.size(1)
            sorted_indices = None
            unsorted_indices = None

        if hx is None:
            num_directions = 2 if self.bidirectional else 1
            hx = torch.zeros(self.num_layers * num_directions,
                             max_batch_size, self.hidden_size,
                             dtype=input.dtype, device=input.device)
        else:
            # Each batch of the hidden state should match the input sequence that
            # the user believes he/she is passing in.
            hx = self.permute_hidden(hx, sorted_indices)

        self.check_forward_args(input, hx, batch_sizes)
        if batch_sizes is None:
            result = _VF.gru(input, hx, self._flat_weights, self.bias, self.num_layers,
                             self.dropout, self.training, self.bidirectional, self.batch_first)
        else:
            result = _VF.gru(input, batch_sizes, hx, self._flat_weights, self.bias,
                             self.num_layers, self.dropout, self.training, self.bidirectional)
        output = result[0]
        hidden = result[1]

        # xxx: isinstance check needs to be in conditional for TorchScript to compile
        if isinstance(orig_input, PackedSequence):
            output_packed = PackedSequence(output, batch_sizes, sorted_indices, unsorted_indices)
            return output_packed, self.permute_hidden(hidden, unsorted_indices)
        else:
            return output, self.permute_hidden(hidden, unsorted_indices)
```
可以看到，前向传播的过程中，出现了2种不同的处理方式，一种是直接输入输出；另一种就是采用`PackedSequence`封装的输入、输出。

## 2.  PackedSequence的实现
[PackedSequence](https://github.com/pytorch/pytorch/blob/c30bc6d4d7b54296179d717c15cd7507ba520bae/torch/nn/utils/rnn.py#L23),继承自
`PackedSequence_`
```
PackedSequence_ = namedtuple('PackedSequence',
                             ['data', 'batch_sizes', 'sorted_indices', 'unsorted_indices'])
```
## 3.  namedtuple介绍

## 4.  __new__, type, metaclass，


## 5.  引申，tensorflow 1的解决思路