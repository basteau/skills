# Acknowledgements

Thanks to everyone whose skills, tools, and guidance this collection builds on.
License terms apply to each contribution, not automatically to the whole collection.

| Author / copyright notice | Contribution | License |
| --- | --- | --- |
| Copyright (c) 2026 [Jakub Krehel](https://github.com/jakubkrehel/skills) | `better-ui` router and accessibility, colors, interface, layout, polish, typography, and writing references | MIT |
| Copyright (c) 2026 [Meng To](https://github.com/MengTo/Skills) | Landing-page strategy adapted into `better-ui` | MIT |
| Copyright (c) 2026 [Elaya](https://github.com/elayadesign/ai-design-skills) | Landing-page design guidance adapted into `better-ui` | MIT |
| Copyright (c) 2026 [Raphael Salaja](https://github.com/raphaelsalaja/userinterface-wiki) | Interaction and visual-design guidance adapted into `better-ui` | MIT |
| Copyright (c) 2026 [Emil Kowalski](https://www.skills.sh/emilkowalski/skills) | `better-ui` animation and PWA references | MIT |
| Copyright (c) 2026 [Lauren Tan / poteto](https://github.com/poteto/plugins/tree/main/pstack) | pstack: `technical-writing`, `unslop`, and TypeScript guidance | MIT |
| Copyright (c) 2026 [Matteo Collina](https://github.com/mcollina/skills) | `typescript-magician` advanced reference and rule files | MIT |
| Copyright (c) 2026 [Matt Pocock](https://github.com/mattpocock/skills) | Workflow skills and agent-writing guidance | MIT |
| Copyright (c) 2026 [Dillon Mulroy](https://github.com/dmmulroy/anti-slop) | `install-anti-slop` | MIT |
| Copyright (c) 2026 [HumanLayer](https://github.com/humanlayer) | `show-me` | MIT |
| Copyright (c) 2023 [Effectful Technologies Inc / Effect-TS](https://github.com/Effect-TS/effect) | Official Effect guidance and API patterns synthesized into `better-effect` | MIT |
| Copyright 2025 [Vercel Inc. / Vercel Labs](https://github.com/vercel-labs) | `agent-browser`; `skills` powers discovery and installation | Apache-2.0 (`agent-browser`) |
| Copyright OpenJS Foundation and other contributors, <www.openjsf.org> | ESLint-derived spacing rules | MIT |
| Copyright (c) 2023-PRESENT [ESLint Stylistic contributors](https://github.com/eslint-stylistic/eslint-stylistic) | Vendored spacing-rule implementation | MIT |

## Further thanks

- [exe.dev / Bold Software](https://github.com/boldsoftware/exe.dev), [Kamal](https://kamal-deploy.org), [Capistrano](https://capistranorb.com), and [Dokku](https://dokku.com): deployment guidance.
- [Kit Langton](https://github.com/kitlangton/skills), [makisuo](https://github.com/makisuo/skills), [mpsuesser](https://github.com/mpsuesser/opencode-effect-enforcer), and [Esteban Marin](https://github.com/EstebanMarin/effect-ts-workshop): Effect research and failure cases.
- [Tailwind Labs](https://tailwindcss.com/docs) and the [Microsoft TypeScript team](https://github.com/microsoft/TypeScript/wiki/Performance): official guidance.
- [better-result / Dillon Mulroy](https://github.com/dmmulroy/better-result), [Valibot](https://valibot.dev), [Standard Schema](https://standardschema.dev), and [Alexis King](https://lexi-lambda.github.io/blog/2019/11/05/parse-don-t-validate/): TypeScript boundary parsing and expected-error guidance.
- `basteau/selfix`: local workflow and deployment experience.

These research sources retain their own terms; credit does not relicense their work.

<details>
<summary>Adaptation notes</summary>

- pstack folders are preserved; local changes scope `unslop`, link companions, and defer indentation to the target project.

- `better-deploy` is original guidance; its research sources and deployment scripts are not bundled.

- `better-effect` synthesizes official Effect patterns and community research, not upstream skill folders or scripts. Source details remain in its references.

- `better-ui` selectively rewrites design skills into topic references rather than preserving upstream folders, catalogs, or demos. Landing-page guidance combines Meng To and Elaya; Tailwind guidance is an original synthesis. Fixed recipes and global mandates are omitted in favor of project conventions.

- `better-ts` adapts poteto's `typescript-best-practices` (`SKILL.md` and `references/patterns.md`) from revision `74dd2291e8e37b12fd6dc49b2acbd655c6bdaf12`. It retains the rule-table/example structure under an explicit-only local name, inlines essential companion-principle guidance, corrects assertion/inference wording, and adds Standard Schema parsing, a Valibot/better-result example, and project-applicable strictness. It replaces the earlier multi-source router and advanced references.

- `typescript-magician` is an unmodified copy of the complete upstream skill folder from Matteo Collina's revision `856efd268ae85482d882f3d0bed869fd020b5c06`, linked from `better-ts` for advanced TypeScript.

- Matt Pocock's `codebase-design`, `grill-me`, `grilling`, `tdd`, and `wait-what` are local adaptations of revision `74ca5fe077456a0b3b2f5310cf9430999fd0b5fd`. The other ten folders are complete upstream copies from `c55ee46073ed923f86ce59a5eb3b6d895095d1b7`.

</details>

Credits and license terms are maintained only in this root file. Include the
applicable notices when redistributing individual skills. Vendored ESLint Stylistic
assets retain their license and provenance beside independently copied code.

<details>
<summary>Required license terms (MIT and Apache-2.0)</summary>

## MIT License

The following terms apply separately to each MIT contribution credited above,
with its corresponding copyright notice.

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

## Apache License 2.0

The following retained license and notice apply to `agent-browser`.

```text
Apache License
                           Version 2.0, January 2004
                        http://www.apache.org/licenses/

TERMS AND CONDITIONS FOR USE, REPRODUCTION, AND DISTRIBUTION

1.  Definitions.

    "License" shall mean the terms and conditions for use, reproduction,
    and distribution as defined by Sections 1 through 9 of this document.

    "Licensor" shall mean the copyright owner or entity authorized by
    the copyright owner that is granting the License.

    "Legal Entity" shall mean the union of the acting entity and all
    other entities that control, are controlled by, or are under common
    control with that entity. For the purposes of this definition,
    "control" means (i) the power, direct or indirect, to cause the
    direction or management of such entity, whether by contract or
    otherwise, or (ii) ownership of fifty percent (50%) or more of the
    outstanding shares, or (iii) beneficial ownership of such entity.

    "You" (or "Your") shall mean an individual or Legal Entity
    exercising permissions granted by this License.

    "Source" form shall mean the preferred form for making modifications,
    including but not limited to software source code, documentation
    source, and configuration files.

    "Object" form shall mean any form resulting from mechanical
    transformation or translation of a Source form, including but
    not limited to compiled object code, generated documentation,
    and conversions to other media types.

    "Work" shall mean the work of authorship, whether in Source or
    Object form, made available under the License, as indicated by a
    copyright notice that is included in or attached to the work
    (an example is provided in the Appendix below).

    "Derivative Works" shall mean any work, whether in Source or Object
    form, that is based on (or derived from) the Work and for which the
    editorial revisions, annotations, elaborations, or other modifications
    represent, as a whole, an original work of authorship. For the purposes
    of this License, Derivative Works shall not include works that remain
    separable from, or merely link (or bind by name) to the interfaces of,
    the Work and Derivative Works thereof.

    "Contribution" shall mean any work of authorship, including
    the original version of the Work and any modifications or additions
    to that Work or Derivative Works thereof, that is intentionally
    submitted to Licensor for inclusion in the Work by the copyright owner
    or by an individual or Legal Entity authorized to submit on behalf of
    the copyright owner. For the purposes of this definition, "submitted"
    means any form of electronic, verbal, or written communication sent
    to the Licensor or its representatives, including but not limited to
    communication on electronic mailing lists, source code control systems,
    and issue tracking systems that are managed by, or on behalf of, the
    Licensor for the purpose of discussing and improving the Work, but
    excluding communication that is conspicuously marked or otherwise
    designated in writing by the copyright owner as "Not a Contribution."

    "Contributor" shall mean Licensor and any individual or Legal Entity
    on behalf of whom a Contribution has been received by Licensor and
    subsequently incorporated within the Work.

2.  Grant of Copyright License. Subject to the terms and conditions of
    this License, each Contributor hereby grants to You a perpetual,
    worldwide, non-exclusive, no-charge, royalty-free, irrevocable
    copyright license to reproduce, prepare Derivative Works of,
    publicly display, publicly perform, sublicense, and distribute the
    Work and such Derivative Works in Source or Object form.

3.  Grant of Patent License. Subject to the terms and conditions of
    this License, each Contributor hereby grants to You a perpetual,
    worldwide, non-exclusive, no-charge, royalty-free, irrevocable
    (except as stated in this section) patent license to make, have made,
    use, offer to sell, sell, import, and otherwise transfer the Work,
    where such license applies only to those patent claims licensable
    by such Contributor that are necessarily infringed by their
    Contribution(s) alone or by combination of their Contribution(s)
    with the Work to which such Contribution(s) was submitted. If You
    institute patent litigation against any entity (including a
    cross-claim or counterclaim in a lawsuit) alleging that the Work
    or a Contribution incorporated within the Work constitutes direct
    or contributory patent infringement, then any patent licenses
    granted to You under this License for that Work shall terminate
    as of the date such litigation is filed.

4.  Redistribution. You may reproduce and distribute copies of the
    Work or Derivative Works thereof in any medium, with or without
    modifications, and in Source or Object form, provided that You
    meet the following conditions:

    (a) You must give any other recipients of the Work or
    Derivative Works a copy of this License; and

    (b) You must cause any modified files to carry prominent notices
    stating that You changed the files; and

    (c) You must retain, in the Source form of any Derivative Works
    that You distribute, all copyright, patent, trademark, and
    attribution notices from the Source form of the Work,
    excluding those notices that do not pertain to any part of
    the Derivative Works; and

    (d) If the Work includes a "NOTICE" text file as part of its
    distribution, then any Derivative Works that You distribute must
    include a readable copy of the attribution notices contained
    within such NOTICE file, excluding those notices that do not
    pertain to any part of the Derivative Works, in at least one
    of the following places: within a NOTICE text file distributed
    as part of the Derivative Works; within the Source form or
    documentation, if provided along with the Derivative Works; or,
    within a display generated by the Derivative Works, if and
    wherever such third-party notices normally appear. The contents
    of the NOTICE file are for informational purposes only and
    do not modify the License. You may add Your own attribution
    notices within Derivative Works that You distribute, alongside
    or as an addendum to the NOTICE text from the Work, provided
    that such additional attribution notices cannot be construed
    as modifying the License.

    You may add Your own copyright statement to Your modifications and
    may provide additional or different license terms and conditions
    for use, reproduction, or distribution of Your modifications, or
    for any such Derivative Works as a whole, provided Your use,
    reproduction, and distribution of the Work otherwise complies with
    the conditions stated in this License.

5.  Submission of Contributions. Unless You explicitly state otherwise,
    any Contribution intentionally submitted for inclusion in the Work
    by You to the Licensor shall be under the terms and conditions of
    this License, without any additional terms or conditions.
    Notwithstanding the above, nothing herein shall supersede or modify
    the terms of any separate license agreement you may have executed
    with Licensor regarding such Contributions.

6.  Trademarks. This License does not grant permission to use the trade
    names, trademarks, service marks, or product names of the Licensor,
    except as required for reasonable and customary use in describing the
    origin of the Work and reproducing the content of the NOTICE file.

7.  Disclaimer of Warranty. Unless required by applicable law or
    agreed to in writing, Licensor provides the Work (and each
    Contributor provides its Contributions) on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or
    implied, including, without limitation, any warranties or conditions
    of TITLE, NON-INFRINGEMENT, MERCHANTABILITY, or FITNESS FOR A
    PARTICULAR PURPOSE. You are solely responsible for determining the
    appropriateness of using or redistributing the Work and assume any
    risks associated with Your exercise of permissions under this License.

8.  Limitation of Liability. In no event and under no legal theory,
    whether in tort (including negligence), contract, or otherwise,
    unless required by applicable law (such as deliberate and grossly
    negligent acts) or agreed to in writing, shall any Contributor be
    liable to You for damages, including any direct, indirect, special,
    incidental, or consequential damages of any character arising as a
    result of this License or out of the use or inability to use the
    Work (including but not limited to damages for loss of goodwill,
    work stoppage, computer failure or malfunction, or any and all
    other commercial damages or losses), even if such Contributor
    has been advised of the possibility of such damages.

9.  Accepting Warranty or Additional Liability. While redistributing
    the Work or Derivative Works thereof, You may choose to offer,
    and charge a fee for, acceptance of support, warranty, indemnity,
    or other liability obligations and/or rights consistent with this
    License. However, in accepting such obligations, You may act only
    on Your own behalf and on Your sole responsibility, not on behalf
    of any other Contributor, and only if You agree to indemnify,
    defend, and hold each Contributor harmless for any liability
    incurred by, or claims asserted against, such Contributor by reason
    of your accepting any such warranty or additional liability.

END OF TERMS AND CONDITIONS

APPENDIX: How to apply the Apache License to your work.

      To apply the Apache License to your work, attach the following
      boilerplate notice, with the fields enclosed by brackets "[]"
      replaced with your own identifying information. (Don't include
      the brackets!)  The text should be enclosed in the appropriate
      comment syntax for the file format. We also recommend that a
      file or class name and description of purpose be included on the
      same "printed page" as the copyright notice for easier
      identification within third-party archives.

Copyright 2025 Vercel Inc.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```

</details>
