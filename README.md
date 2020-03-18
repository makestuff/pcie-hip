## Altera PCI-Express hard-IP
You can git submodule this repo to provide user-friendly layer on top of th Intel/Altera PCI-Express hard-IP for Cyclone V and Stratix V.

**Dependencies:** [`makestuff:altera-libs`](https://github.com/makestuff/altera-libs) [`makestuff:buffer-fifo`](https://github.com/makestuff/buffer-fifo) [`makestuff:block-ram`](https://github.com/makestuff/block-ram) [`makestuff:dvr-rng`](https://github.com/makestuff/dvr-rng)

See [BuildInfra](https://github.com/makestuff/ws-tools/blob/master/README.md) for details of how to incorporate this into your project, and [`altera-pcie`](https://github.com/makestuff/altera-pcie) repo for an example project repo which incorprates submodules of this repo and its dependencies.

You can install it in a new workspace `$HOME/my-workspace` like this:

    cd $HOME
    export ALTERA=/usr/local/altera-16.1  # or wherever
    mkws.sh my-workspace makestuff:{altera-libs,buffer-fifo,block-ram,dvr-rng,pcie-hip}
    export PROJ_HOME=$HOME/my-workspace

Then assuming you have ModelSim in your `PATH`, you can run all the tests:

    make -C $PROJ_HOME/ip test

You need to add either the [`cyclonev`](cyclonev/pcie.qip) or the [`stratixv`](stratixv/pcie.qip) QIP file to your project's `.qsf`. This will import both the FPGA-specific hard-IP and the FPGA-agnostic [TLP transceiver](tlp-xcvr). You will also need a (possibly modified) copy of the [`defs.vh`](example-defs/defs.vh) file in your project directory, and a Makefile rule similar to this:

    defs: FORCE
          cat defs.vh | sed 's/localparam int/#define/g' | sed 's/=//g' | sed 's/;//g' | sed 's/1<</1U<</g;s/\((\d+)\)/\(\1U\)/g' > ${PROJ_HOME}/pcie-driver/defs.h

...which will take your SystemVerilog `defs.vh` file and produce a C/C++ `defs.h` file needed in order to build the [Linux driver](https://github.com/makestuff/pcie-driver), which must be [git submoduled](https://git-scm.com/book/en/v2/Git-Tools-Submodules) at `$PROJ_HOME/pcie-driver`.

There is also an example consumer block, which (along with a suitable block-RAM such as [`makestuff_ram_sc_be`](https://github.com/makestuff/block-ram/blob/master/ram_sc_be.sv)) can be wired up with the TLP transceiver in order to consume (at a programmable rate) the stream of data coming from the CPU->FPGA circular-buffer, keeping a 64-bit checksum, allowing the CPU to verify that all its data has been consumed. Although of no practical use, you can use this as a basis for your own consumer logic. See the [demo app](https://github.com/makestuff/altera-pcie/blob/master/apps/demo/pcie_app.sv#L77-L93) for guidance.
