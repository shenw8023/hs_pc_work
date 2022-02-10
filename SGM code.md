```
def build_model():
	embedding = get_embedding(checkpoints)
	model = getattr(models, opt.model)(config, embedding=embedding)

	if checkpoint:
		model.load_state_dict(checkpoints['model'])

	# optimizer
	optim = models.Optim(config.optim, ...)
	optim.set_parameters(model.parameters())


	

class seq2seq(nn.Module):

	def __init__(self):
		self.encoder = models.rnn_encoder(config, embedding)
		self.decoder = models.rnn_decoder(config, embedding=tgt_embedding)
		self.criterion = nn.CrossEntropyLoss()

	def forward(self,src, src_len, dec, targets):
		outputs = []
	    output = None
	
	    for input in dec.split(1):
		    output, state, _ = self.decoder(input.squeeze(0), state, output)
			outputs.append(output)
			outputs = torch.stack(outputs)
		loss = self.compute_loss(outputs, targets)
		return loss, outputs

```