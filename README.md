PLPatchMaster
-----------

PLPatchMaster provides an easy-to-use block-based swizzling API, using the block trampoline
library provided by PLBlockIMP and a set of custom assembly trampolines for ARM, ARM64, i386,
and x86-64.

Use it at your own risk; swizzling in production software is rarely, if ever, a great idea.

PLPatchMaster is released under the MIT license.

Sample Use
-----------

Use a block to swizzle -[NSObject description]:

    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        [NSObject ex_patchInstanceSelector: @selector(description) withReplacementBlock: ^(EXPatchIMP *patch) {
            NSObject *obj = EXPatchGetSelf(patch);
            NSString *defaultDescription = EXPatchIMPFoward(patch, NSString *(*)(id, SEL));
            return [NSString stringWithFormat: @"Generated description for %p: %@", obj, defaultDescription];
        }];
    });