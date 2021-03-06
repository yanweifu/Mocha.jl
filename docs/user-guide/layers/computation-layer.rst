Computation Layers
~~~~~~~~~~~~~~~~~~

.. class:: InnerProductLayer

   Densely connected linear layer. The output is computed as

   .. math::

      y_i = \sum_j w_{ij}x_j + b_i

   where :math:`w_{ij}` are the weights and :math:`b_i` are bias.

   .. attribute:: output_dim

      Output dimension of the linear map. The input dimension is automatically
      decided via the inputs.

   .. attribute:: weight_init

      Default ``XavierInitializer()``. Specify how the weights :math:`w_{ij}` should
      be initialized.

   .. attribute:: bias_init

      Default ``ConstantInitializer(0)``, initializing the bias :math:`b_i`
      to 0.

   .. attribute:: weight_regu

      Default ``L2Regu(1)``. :doc:`Regularizer </user-guide/regularizer>` for the weights.

   .. attribute:: bias_regu

      Default ``NoRegu()``. Regularizer for the bias. Typically no
      regularization should be applied to the bias.

   .. attribute:: weight_lr

      Default 1.0. The local learning rate for the weights.

   .. attribute:: bias_lr

      Default 2.0. The local learning rate for the bias.

   .. attribute:: neuron

      Default ``Neurons.Identity()``, an optional :doc:`activation function
      </user-guide/neuron>` for the output of this layer.

   .. attribute::
      tops
      bottoms

      Blob names for output and input.

.. class:: PoolingLayer

   2D pooling over the 2 image dimensions (width and height).

   .. attribute:: kernel

      Default (1,1), a 2-tuple of integers specifying pooling kernel width and
      height, respectively.

   .. attribute:: stride

      Default (1,1), a 2-tuple of integers specifying pooling stride in the
      width and height dimensions respectively.

   .. attribute:: pad

      Default (0,0), a 2-tuple of integers specifying the padding in the width and
      height dimensions respectively. Paddings are two-sided, so a pad of (1,0)
      will pad one pixel in both the left and the right boundary of an image.

   .. attribute:: pooling

      Default ``Pooling.Max()``. Specify the pooling operation to use.

   .. attribute::
      tops
      bottoms

      Blob names for output and input.

.. class:: LRNLayer

   Local Response Normalization Layer. It performs normalization over local
   input regions via the following mapping

   .. math::

      x \rightarrow y = \frac{x}{\left( \beta + (\alpha/n)\sum_{x_j\in N(x)}x_j^2
      \right)^p}

   Here :math:`\beta` is the shift, :math:`\alpha` is the scale, :math:`p` is
   the power, and :math:`n` is the size of the local neighborhood. :math:`N(x)`
   denotes the local neighborhood of :math:`x` of size :math:`n` (including
   :math:`x` itself). There are two types of local neighborhood:

   * ``LRNMode.AcrossChannel()``: The local neighborhood is a region of shape
     (1, 1, :math:`k`, 1) centered at :math:`x`. In other words, the region
     extends across nearby channels (with zero padding if needed), but has no
     spatial extent. Here :math:`k` is the kernel size, and :math:`n=k` in this
     case.
   * ``LRNMode.WithinChannel()``: The local neighborhood is a region of shape
     (:math:`k`, :math:`k`, 1, 1) centered at :math:`x`. In other words, the
     region extends spatially (in **both** the width and the channel dimension),
     again with zero padding when needed. But it does not extend across
     different channels. In this case :math:`n=k^2`.

   .. attribute:: kernel

      Default 5, an integer indicating the kernel size. See :math:`k` in the
      descriptions above.

   .. attribute:: scale

      Default 1.

   .. attribute:: shift

      Default 1 (yes, 1, not 0).

   .. attribute:: power

      Default 0.75.

   .. attribute:: mode

      Default ``LRNMode.AcrossChannel()``.

   .. attribute::
      tops
      bottoms

      Names for output and input blobs. Only one input and one output blob are
      allowed.


.. class:: ElementWiseLayer

   Element-wise layer implements basic element-wise operations on inputs.

   .. attribute:: operation

      Element-wise operation. Built-in operations are in module
      ``ElementWiseFunctors``, including ``Add``, ``Subtract``, ``Multiply`` and
      ``Divide``.

   .. attribute:: tops

      Output blob names, only one output blob is allowed.

   .. attribute:: bottoms

      Input blob names, count must match the number of inputs ``operation`` takes.

.. class:: PowerLayer

   Power layer performs element-wise operations as

   .. math::

     y = (ax + b)^p

   where :math:`a` is ``scale``, :math:`b` is ``shift``, and :math:`p` is
   ``power``. During back propagation, the following element-wise derivatives are
   computed:

   .. math::

     \frac{\partial y}{\partial x} = pa(ax + b)^{p-1}

   Power layer is implemented separately instead of as an Element-wise layer
   for better performance because there are some many special cases of Power layer that
   could be computed more efficiently.

   .. attribute:: power

      Default 1

   .. attribute:: scale

      Default 1

   .. attribute:: shift

      Default 0

   .. attribute::
      tops
      bottoms

      Blob names for output and input.

.. class:: SplitLayer

   Split layer produces identical *copies* [1]_ of the input. The number of copies
   is determined by the length of the ``tops`` property. During back propagation,
   derivatives from all the output copies are added together and propagated down.

   This layer is typically used as a helper to implement some more complicated
   layers.

   .. attribute:: bottoms

      Input blob names, only one input blob is allowed.

   .. attribute:: tops

      Output blob names, should be more than one output blobs.

   .. [1] All the data is shared, so there is no actually data copying.

.. class:: ChannelPoolingLayer

   1D pooling over the channel dimension.

   .. attribute:: kernel

      Default 1, pooling kernel size.

   .. attribute:: stride

      Default 1, stride for pooling.

   .. attribute:: pad

      Default (0,0), a 2-tuple specifying padding in the front and the end.

   .. attribute:: pooling

      Default ``Pooling.Max()``. Specify the pooling function to use.

   .. attribute::
      tops
      bottoms

      Blob names for output and input.

