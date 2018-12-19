# README

## Description

This project is a simple implementation of multi-layered block-wise disk storage system using C.

The disk layer is the base of the system. There could be multiple layers on top of it. Among the layers, there are:

### Functional layers

1. Trace layer: run as a top layer and simply replays a trace of read and write operations from a file.

2. Debug layer: simply forwards its method calls to an underlying layer, but prints out invocations and returns for debugging.

3. Statistics layer: simply forwards its method calls to an underlying layer, but keeps track of the underlying layer's statistics (reads, writes, etc.).

4. Check layer: simply forwards its method calls to an underlying layer, but checks whether reads and prior writes are synchronous.

### Structural layers

1. Tree layer: this layer simulates the tree-like hierarchical inode storage system (like UNIX FFS).

2. Cache layer: this layer simulates the write-through cache structure.

3. RAM layer: this layer simulates the RAM memory structure.

===================================================================================================================

All layers are block store objects (defined in block_store.h). A block store has the following interface:

## Block store interface

    #include "block_store.h"
		Defines BLOCK_SIZE, the size of a block.  Also has typedefs
		for:
			block_store_t: a block store interface
			block_t:  a block of size BLOCK_SIZE
			block_no: an offset into a block store

	int nblocks = (*block_store->nblocks)(block_store_t *block_store);
		Returns the size of the block store in #blocks, or -1 if error.

	int (*block_store->read)(block_store, block_no offset, OUT block_t *block);
		Reads the block at the given offset.  Return 0 upon success,
		-1 upon error.

	int (*block_store->write)(block_store, block_no offset, IN block_t *block);
		Writes the block at the given offset.  Some block stores support
		automatically growing.  Returns -1 upon error.

	int (*block_store->setsize)(block_store, block_no size);
		Set the size of the block store to 'size' blocks.  May either
		truncate or grow the underlying block store.  Not all sizes
		may be supported.  Returns the old size, or -1 upon error.

	void (*block_store->destroy)(block_store);
		Clean up the block store.  In case of a low-level storage (an
		actual disk), will typically leave the blocks intact so it can
		be reloaded again.

## Acknowledgement

This project is modified from the idea of Cornell University CS4410 SP2018 assignment 4, by Prof. Robert Van Renesse.
