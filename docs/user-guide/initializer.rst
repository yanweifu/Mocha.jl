Initializers
============

Initializers provide init values for network parameter blobs. In Caffe, they are
called *Fillers*.

.. class:: NullInitializer

   An initializer that does nothing.

.. class:: ConstantInitializer

   Set everything to a constant.

   .. attribute:: value

      The value used to initialize a parameter blob. Typically this is set to 0.

.. class:: XavierInitializer

   An initializer based on [BengioGlorot2010]_, but does not use the fan-out
   value. It fills the parameter blob by randomly sampling uniform data from
   :math:`[-S,S]` where the scale :math:`S=\sqrt{3 / F_{\text{in}}}`. Here
   :math:`F_{\text{in}}` is the fan-in: the number of input nodes. For a 4D
   tensor parameter blob with the shape :math:`(M,N,P,Q)`, :math:`M` is
   considered as the fan-in.

   .. [BengioGlorot2010] Y. Bengio and X. Glorot, *Understanding the
      difficulty of training deep feedforward neural networks*, in Proceedings of
      AISTATS 2010, pp. 249-256.

.. class:: GaussianInitializer

   Initialize each element in the parameter blob as independent and identically
   distributed Gaussian random variables.

   .. attribute:: mean

      Default 0.

   .. attribute:: std

      Default 1.
