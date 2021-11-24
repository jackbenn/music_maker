# Music Maker

Tool to generate music based on personal training using reinforcement learning (RL).

This uses would use a recurrent neural network (RNN) that will create musical notes at each time point, using prior notes as input to the network.

The output of the network at each time step is an NxM array of q-values (if we use Q-learning), with the N dimension representing the pitch, and the M dimension representing the continuation/duration of the note.

The N dimension is measured in half-steps, with some reasonable number of values. (64?88? 128?). At each time step, for each pitch, a the continuation/duration value is choosen (perhaps using epsilon-greedy or some other approach for exploration); this will describe the output at that moment.

I'm considering two approaches for continuation/duration.

One option is the different output nodes representing different durations (1, 2, 3, or 4 time steps) for a notes, plus none for change. If that pitch is currently playing and a non-none value is selected, it is ended and a new note starts of the given length.

The other option is that different notes represent different continuation choices, that map silence (of that frequency) to silence or a note, and a note to silence, continuation, or restarting the noting. That leaves six possible options; we may not include them all (toggle is a little weird). We really only need three of them (one of each option for if silent and if playing).

Number | Name     | Result if silent? | Result if playing?
-------|----------|-------------------|--------------------
1      | off      | off               | off
2      | continue | off               | continue
3      | restart  | off               | restart
4      | toggle   | on                | off
5      | on       | on                | continue
6      | play     | on                | restart

## Interface

The interface would play the song; the user would hit keys (1-9) to describe their level of satisfaction at that point, with the satisfaction level staying constant until a new ket was struck. This would be used as the reward function.
