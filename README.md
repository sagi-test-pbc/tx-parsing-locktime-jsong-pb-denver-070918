
# Parsing Locktime from a Transaction

Locktime is a way to time-delay a transaction. A transaction with a locktime of 525,000 cannot go into the blockchain until block 525,000. this was originally construed as a way to do payment channels (see sidebar). The rule with locktime is that if the locktime is greater than 500,000,000, it is a unix time stamp. If it is less than 500,000,000, it is a block number. This way, transactions can be signed, but unspendable until a certain point in time or block.

### Test Driven Exercise


```python
from io import BytesIO
from helper import (
    little_endian_to_int,
    read_varint,
)
from tx import Tx, TxIn, TxOut

class Tx(Tx):

    @classmethod
    def parse(cls, s):
        '''Takes a byte stream and parses the transaction at the start
        return a Tx object
        '''
        # s.read(n) will return n bytes
        # version has 4 bytes, little-endian, interpret as int
        version = little_endian_to_int(s.read(4))
        # num_inputs is a varint, use read_varint(s)
        num_inputs = read_varint(s)
        # each input needs parsing
        inputs = []
        for _ in range(num_inputs):
            inputs.append(TxIn.parse(s))
        # num_outputs is a varint, use read_varint(s)
        num_outputs = read_varint(s)
        # each output needs parsing
        outputs = []
        for _ in range(num_outputs):
            outputs.append(TxOut.parse(s))

        # locktime is 4 bytes, little-endian
        # return an instance of the class (cls(...))
        pass
```
