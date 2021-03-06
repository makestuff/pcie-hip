## Stratix V PCI-Express hard-IP
The low-level PCI-Express hard-IP for Stratix V, provided by Altera.

The `pcie_sv.qsys` QSys design was based on an Altera PCIe design example. To create it, I just did this:

    cp $ALTERA/ip/altera/altera_pcie/altera_pcie_hip_ast_ed/example_design/sv/pcie_de_gen1_x4_ast64.qsys pcie_sv.qsys
    $ALTERA/quartus/sopc_builder/bin/qsys-edit --family="Stratix V" pcie_sv.qsys

Then, in QSys:
  * Delete `pcie_reconfig_driver_0` (i.e select it and hit <Del>).
  * Delete `alt_xcvr_reconfig_0`.
  * Right-click `DUT` and select "Edit".
  * Set BAR0 to 4KiB of 32-bit non-prefetchable memory.
  * Disable BAR2.
  * Ensure the Vendor ID is `0x1172`, the Device ID is `0xE001`, and the Class Code is `0x00FF0000`.
  * Click "Finish".
  * Click "File->Save".
  * Click "Close".
  * Click "File->Exit".
  * Click "No" (i.e don't generate now).
