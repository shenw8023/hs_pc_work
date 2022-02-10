
## torch.nn.LSTM()
- [官方文档](https://pytorch.org/docs/stable/generated/torch.nn.LSTM.html?highlight=lstm#torch.nn.LSTM)
- 普通单向lstm
```python
rnn = nn.LSTM(input_size=10, hidden_size=20, num_layers=2)
input = torch.randn(5, 3, 10)
h0 = torch.randn(2, 3, 20)
c0 = torch.randn(2, 3, 20)
output, (hn, cn) = rnn(input, (h0, c0))

print(output.size())  # torch.Size([5, 3, 20])
print(hn.size())  # torch.Size([2, 3, 20])
print(cn.size()) # torch.Size([2, 3, 20])
```

- 双向lstm
```python
rnn = nn.LSTM(input_size=10, hidden_size=20, num_layers=2, bidirectional=True)
input = torch.randn(5, 3, 10)
h0 = torch.randn(2*2, 3, 20)
c0 = torch.randn(2*2, 3, 20)
output, (hn, cn) = rnn(input, (h0, c0))

print(output.size()) # torch.Size([5, 3, 40])
print(hn.size())  # torch.Size([4, 3, 20])
print(cn.size()) # torch.Size([4, 3, 20])
```

- 总结
	- num_layers 控制多少层的lstm进行堆叠，对应h0, c0, hn, cn 的第一个维度大小，如果是bidirectional，第一个维度再乘2
	- bidirectional = True，影响是增加了h0, c0, hn, cn的第一维度大小，output的隐层维度乘2，说明output最终双向的结果是拼接在一起的
	- output输出包含每一个时间步的输出
	- hn, cn仅仅是最后一步的




## torch.nn.LSTMCell()
- 单个时间步的lstm
- [官方文档](https://pytorch.org/docs/stable/generated/torch.nn.LSTMCell.html?highlight=nn%20lstmcell#torch.nn.LSTMCell)

```python
rnn = nn.LSTMCell(10, 20) # (input_size, hidden_size)

input = torch.randn(2, 3, 10) # (time_steps, batch, input_size)
hx = torch.randn(3, 20) # (batch, hidden_size)
cx = torch.randn(3, 20)

output = []

for i in range(input.size()[0]):
	hx, cx = rnn(input[i], (hx, cx))
	output.append(hx)
	
output = torch.stack(output, dim=0)
```
