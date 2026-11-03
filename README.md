# Ultra Horizon's asio fork for UH VPN

This repository is Ultra Horizon's fork of [asio](https://github.com/chriskohlhoff/asio),
for use by the UH VPN applications. It contains an upstream asio release plus
the patch set that [OpenVPN 3](https://github.com/ultrahorizon/openvpn3)
requires, as a real git tree. Swift Package Manager and CMake can then use the
patched headers directly, instead of patching asio at build time.

The patches are OpenVPN 3's, not Ultra Horizon's. They are the ones in the
openvpn3 fork's vcpkg port (`deps/vcpkg-ports/asio`), applied unchanged with
`git am`, so each is a separate commit that keeps its original author and
message. Nothing else is changed apart from this README.

**This fork is never to be upstreamed.** Nothing on it should be pushed or
proposed back to upstream asio.

The following branches are noteworthy:

* `uh-vpn-master` is the default branch in this fork. It contains an upstream
  asio release with OpenVPN 3's asio patches on top. UH VPN repositories pin
  commits from this branch as a submodule.
* `master` tracks the upstream asio repository, unchanged.

## Git Fork Technical Notes

`uh-vpn-master` currently starts from asio 1.38.2 (`8806a68`). To see the
fork's commits on top of upstream:

    $ git log --oneline $(git merge-base uh-vpn-master master)..uh-vpn-master

To see the complete set of changes relative to upstream as a diff:

    $ git diff $(git merge-base uh-vpn-master master) uh-vpn-master

Do not rewrite `uh-vpn-master`. Other repositories pin its commits, and a
rewrite would leave those pins unreachable. When OpenVPN 3 moves to a newer
asio or changes its patches, merge the new upstream release into
`uh-vpn-master` and bring the patches in line with openvpn3's
`deps/vcpkg-ports/asio`. Then check that the diff against upstream matches
that patch set.

*The original asio README is in [`README`](README).*
