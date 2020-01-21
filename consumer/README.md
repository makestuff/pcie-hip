## Example consumer
This is an example consumer for the CPU->FPGA circular-buffer.

**Dependencies:** [`makestuff/block-ram`](https://github.com/makestuff/block-ram).

It can consume data given it by the CPU at a programmable rate, whilst maintaining a 64-bit checksum that can be used for verification purposes. Although of no practical use, you can use this as a basis for your own consumer logic. See the [demo app](https://github.com/makestuff/altera-pcie/blob/master/apps/demo/pcie_app.sv#L77-L93) for guidance.
