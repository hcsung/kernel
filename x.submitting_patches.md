# Submitting Patches

> Unfinished

Kindly Reminder, you are in **danger zone**, please read this page carefully before sending the patches, or reviewers could be very upset (believe me, I made noob mistakes as well, and that's the reason why I wrote this page).

To avoid writing too many things and increase the chance to miss the really important ones, I'll try to keep it short and share my checklist at the beginning.

Before starting, keep in mind that, take your first patch seriously, **DO NOT** attempt to submit your first version as a draft to collect reviewers' comments.



## Checklist

1. [Fix typos](#fix-typos)
2. [Build kernel](#build-kernel)
3. [Check dt-bindings](#check-dt-bindings)
   - `dt_binding_check`, `dtbs_check` and `yamllint`
4. [Create patches](#create-patches)
   - [Add Acked-by/Reviewed-by/Tested-by](#add-acked-byreviewed-bytested-by) ([b4](https://b4.docs.kernel.org/en/latest/index.html))
   - [Add version number](#add-version-number)
   - [Remove Change-Id](#remove-change-id)
   - [Run `checkpatch.pl`](#run-checkpatchpl)
5. [Submit](#submit)
   - [Get maintainer list](#get-maintainer-list)
   - [Send mail](#send-mail)
6. [Others](#others)
   - [Script](#script)
   - [Suggestions](#suggestions)



## Fix typos

Be ware of that sometimes spelling check of your editor may not work properly and make you think there is no typo. For example, my [vscode](https://code.visualstudio.com/) spelling check stops functioning with file extensions like `.yaml` or `.md`. There should be more robust ways to check spelling, will update if I found one.



## Build kernel

Make sure kernel can boot and function well with the new patches.



## Check dt-bindings

```sh
make dt_binding_check
```

```sh
make dtbs_check
```

```sh
yamllint
```



## Create patches

Run Git command to transform your commits into `.patch` files:

```sh
git format-patch -$COUNT --cover-letter
```

```sh
# git format-patch -3 --cover-letter
0000-cover-letter.patch
0001-octeon_ep-Add-missing-check-for-ioremap.patch
0002-udplite-Print-deprecation-notice.patch
0003-dccp-Print-deprecation-notice.patch
```

### Add Acked-by/Reviewed-by/Tested-by

You will receive some of the following tags in mailing list once the reviewer or tester accept your patches, these tags should be added to the patches before or after your `Signed-off-by:` tag when sending next versions ([reference](https://elixir.bootlin.com/linux/v5.17/source/Documentation/process/submitting-patches.rst#L540)).

* [Acked-by](https://www.kernel.org/doc/html/v4.17/process/submitting-patches.html#when-to-use-acked-by-cc-and-co-developed-by): A record that the acker has at least reviewed the patch and has indicated acceptance
* [Reviewed-by](https://www.kernel.org/doc/html/v4.17/process/submitting-patches.html#using-reported-by-tested-by-reviewed-by-suggested-by-and-fixes): The patch has been reviewed and found acceptable according to the reviewer
* [Tested-by](https://www.kernel.org/doc/html/v4.17/process/submitting-patches.html#using-reported-by-tested-by-reviewed-by-suggested-by-and-fixes): The patch has been successfully tested (in some environment) by the person named.

There is a [b4](https://b4.docs.kernel.org/en/latest/index.html) tool that can do the jobs for you ([how I know that](https://lore.kernel.org/all/eedf5cea-84e9-7d3d-856b-448912417b60@linaro.org/)):

```sh
b4 am $MSG_ID
```

```sh
b4 am 20230621031938.5884-1-shawn.sung@mediatek.com

Analyzing 23 messages in the thread
Checking attestation on all messages, may take a moment...
---
  ✓ [PATCH v4 1/14] dt-bindings: display: mediatek: ethdr: Add compatible for MT8188
  ✓ [PATCH v4 2/14] dt-bindings: display: mediatek: mdp-rdma: Add compatible for MT8188
  ✓ [PATCH v4 3/14] dt-bindings: display: mediatek: merge: Add compatible for MT8188
  ✓ [PATCH v4 4/14] dt-bindings: display: mediatek: padding: Add MT8188
    + Reviewed-by: Krzysztof Kozlowski <krzysztof.kozlowski@linaro.org> (✓ DKIM/linaro.org)
  ✓ [PATCH v4 5/14] dt-bindings: arm: mediatek: Add compatible for MT8188
  ✓ [PATCH v4 6/14] dt-bindings: reset: mt8188: Add VDOSYS reset control bits
    + Acked-by: Krzysztof Kozlowski <krzysztof.kozlowski@linaro.org> (✓ DKIM/linaro.org)
  ✓ [PATCH v4 7/14] soc: mediatek: Support MT8188 VDOSYS1 in mtk-mmsys
  ✓ [PATCH v4 8/14] soc: mediatek: Support MT8188 VDOSYS1 Padding in mtk-mmsys
  ✓ [PATCH v4 9/14] soc: mediatek: Support reset bit mapping in mmsys driver
    + Reviewed-by: AngeloGioacchino Del Regno <angelogioacchino.delregno@collabora.com> (✓ DKIM/collabora.com)
  ✓ [PATCH v4 10/14] soc: mediatek: Add MT8188 VDOSYS reset bit map
    + Reviewed-by: AngeloGioacchino Del Regno <angelogioacchino.delregno@collabora.com> (✓ DKIM/collabora.com)
  ✓ [PATCH v4 11/14] drm/mediatek: Support MT8188 VDOSYS1 in display driver
  ✓ [PATCH v4 12/14] drm/mediatek: Improve compatibility of display driver
  ✓ [PATCH v4 13/14] drm/mediatek: Sort OVL adaptor components in alphabetical order
  ✓ [PATCH v4 14/14] drm/mediatek: Support MT8188 Padding in display driver
  ---
  ✓ Signed: DKIM/mediatek.com
---
Total patches: 14
---
Cover: ./v4_20230621_shawn_sung_add_display_driver_for_mt8188_vdosys1.cover
 Link: https://lore.kernel.org/r/20230621031938.5884-1-shawn.sung@mediatek.com
 Base: applies clean to current tree
       git checkout -b v4_20230621_shawn_sung_mediatek_com HEAD
       git am ./v4_20230621_shawn_sung_add_display_driver_for_mt8188_vdosys1.mbx
```

Notice that you have to run `git am ./v4_20230621_shawn_sung_add_display_driver_for_mt8188_vdosys1.mbx` again to apply the patches.

### Add version number

In any `.patch` file you just created, the subject should look like:

```
Subject: [PATCH 1/3] some_tag: Add foo bar driver
```

Add `v2` if it's your second version and increase it accordingly:

```
Subject: [PATCH v2 1/3] some_tag: Add foo bar driver
```

### Remove Change-Id

A standard commit-msg hook will be generated on the client side if you are using Gerrit. But currently there is no "official" story for gerrit in the linux process ([reference](https://lore.kernel.org/workflows/CACT4Y+Yezqn1JZac9=R1dmgZ4d6N1btAsP6AHON-Cds1knTaiA@mail.gmail.com/t/)), so we gotta remove it from every `.patch` file.

```diff
-Change-Id: I1733fb5f6569c44a918696d350e45d246d51d2ad
 Signed-off-by: Foo Bar (foo@bar.com)
```

### Run `checkpatch.pl`

```sh
./scripts/checkpatch.pl --strict *.patch
```

```
-----------------------
0000-cover-letter.patch
-----------------------
total: 0 errors, 0 warnings, 0 lines checked

0000-cover-letter.patch has no obvious style problems and is ready for submission.
----------------------------------------
0001-dccp-Print-deprecation-notice.patch
----------------------------------------
WARNING: quoted string split across lines
#35: FILE: net/dccp/proto.c:195:
+       pr_warn_once("DCCP is deprecated and scheduled to be removed in 2025, "
+                    "please contact the netdev mailing list\n");

total: 0 errors, 1 warnings, 0 checks, 9 lines checked

NOTE: For some of the reported defects, checkpatch may be able to
      mechanically convert to the typical style using --fix or --fix-inplace.

0001-dccp-Print-deprecation-notice.patch has style problems, please review.

NOTE: If any of the errors are false positives, please report
      them to the maintainer, see CHECKPATCH in MAINTAINERS.
```



## Submit

### Get maintainer list

```sh
./scripts/get_maintainer.pl *.patch
```

```
David S. Miller" <davem@davemloft.net> (maintainer:NETWORKING [GENERAL])
Eric Dumazet <edumazet@google.com> (maintainer:NETWORKING [GENERAL],commit_signer:2/6=33%)
Jakub Kicinski <kuba@kernel.org> (maintainer:NETWORKING [GENERAL],commit_signer:5/6=83%)
Paolo Abeni <pabeni@redhat.com> (maintainer:NETWORKING [GENERAL])
Kuniyuki Iwashima <kuniyu@amazon.com> (commit_signer:4/6=67%,authored:4/6=67%,added_lines:13/47=28%,removed_lines:3/13=23%)
Joanne Koong <joannelkoong@gmail.com> (commit_signer:2/6=33%,authored:1/6=17%,added_lines:29/47=62%,removed_lines:5/13=38%)
Hangyu Hua <hbh25y@gmail.com> (commit_signer:1/6=17%,authored:1/6=17%,added_lines:5/47=11%,removed_lines:5/13=38%)
dccp@vger.kernel.org (open list:DCCP PROTOCOL)
netdev@vger.kernel.org (open list:NETWORKING [GENERAL])
linux-kernel@vger.kernel.org (open list)
```

### Send mail

Send patches to maintainers you got by `get_maintainer.pl` and cc to any one you want:

```sh
git send-email --compose --annotate \
--to "David S. Miller <davem@davemloft.net>" \
--to "Eric Dumazet <edumazet@google.com>" \
--cc "Any One <to-any-one@you.want>" \
*.patch
```

```diff
 From Foo Bar <foo@bar.com> # This line is ignored.
 GIT: Lines beginning in "GIT:" will be removed.
 GIT: Consider including an overall diffstat or table of  contents
 GIT: for the patch you are writing.
 GIT:
 GIT: Clear the body content if you don't wish to send a  summary.
 From: Foo Bar <foo@bar.com>
 Reply-To:
-Subject:
+Subject: Add foo bar function to driver
 In-Reply-To:

 GIT: [PATCH 0/1] Add foo bar function to driver
 GIT: [PATCH 1/1] tag: Support foo bar function
```



## Others

### Script

```sh
#!bin/sh

TARGETS="*.patch"

# remove change id
sed -i '/Change-Id:.*/d' $TARGETS

# add version number if any
[ "x$1" != "x" ] && sed -i \
    "s/\(\[PATCH\).*\( [0-9]\)/\1 $1\2/" $TARGETS

# run kernel check patch script
scripts/checkpatch.pl $TARGETS
```

### Suggestions

* Coding
  - Try to sort everything in your code in alphabetical order
  - Add [documentation](https://www.kernel.org/doc/html/latest/doc-guide/kernel-doc.html) for your functions and structures
  - Follow [coding style](https://kernel.org/doc/html/latest/process/coding-style.html)
* Make every commit as formal as it could be
  - A nit and meaningful title
  - Clear and easy to understand commit message
* Reply the mailing list inline
  - Usually we reply email above the quoted original message, but it's different here. Add your reply below the section you'd like to response. ([example](https://lore.kernel.org/all/643e6681-6ba7-e990-3e90-09071db904d2@linaro.org/))

### Case study

* [Example 1](https://lore.kernel.org/all/e4f98dc5-0fa6-14aa-f8d0-e4bf30ecca5c@collabora.com/): Use MACRO to improve readibility
* [Example 2](https://lore.kernel.org/all/0db4a705-10d0-93ce-df61-1a20d6d09959@collabora.com/): Every commit has to work on its own



## Reference

- [Documentation/process/submitting-patches](https://www.kernel.org/doc/html/latest/process/submitting-patches.html)
- [Documentation/process/submit-checklist](https://www.kernel.org/doc/html/latest/process/submit-checklist.html)
