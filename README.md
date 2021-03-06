[![PyPI version](https://badge.fury.io/py/chester.svg)](https://badge.fury.io/py/chester)
[![Downloads](https://pepy.tech/badge/chester)](https://pepy.tech/project/chester)
[![Project Status: Active – The project has reached a stable, usable state and is being actively developed.](http://www.repostatus.org/badges/latest/active.svg)](http://www.repostatus.org/#active)
[![Build Status](https://travis-ci.org/bsamseth/python-chess-engine-tester.svg?branch=master)](https://travis-ci.org/bsamseth/python-chess-engine-tester)
[![Language grade: Python](https://img.shields.io/lgtm/grade/python/g/bsamseth/python-chess-engine-tester.svg?logo=lgtm&logoWidth=18)](https://lgtm.com/projects/g/bsamseth/python-chess-engine-tester/context:python)
[![Total alerts](https://img.shields.io/lgtm/alerts/g/bsamseth/python-chess-engine-tester.svg?logo=lgtm&logoWidth=18)](https://lgtm.com/projects/g/bsamseth/python-chess-engine-tester/alerts/)
[![Lines of Code](https://tokei.rs/b1/github/bsamseth/python-chess-engine-tester)](https://github.com/Aaronepower/tokei)
[![license](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/bsamseth/python-chess-engine-tester/blob/master/LICENSE)

# Chester - Chess Engine Tester

`chester` is a Python package that aims to provide a simple interface for running chess matches
between computer chess programs (referred to as engines). It makes it easy to
play 1v1 matches of any length, including running tournaments between a large
set of engines.

## Features

- Easily setup tournaments between chess engines
- Arbitrary time controls
- Support for using polyglot opening books
- Support for arbitration of games, for drawing long games and resigning lost positions (__TODO__)

## Example Usage

The following example shows how to run a 2-player tournament between
[Stockfish](https://github.com/official-stockfish/Stockfish) and
[Goldfish](https://github.com/bsamseth/Goldfish). All players must be UCI compatible
chess engines.

```python
from chester.timecontrol import TimeControl
from chester.tournament import play_tournament

# Each string is the name/path to an executable UCI engine.
players = ["stockfish", "goldfish"]

# Specify time and increment, both in seconds.
time_control = TimeControl(initial_time=2, increment=0)

# Play each math-up twice.
n_games = 2

for pgn in play_tournament(
    players,
    time_control,
    n_games=n_games,
    opening_book="openingbooks/gm2001.bin",  # Optionally use an opening book.
    repeat=True,  # Each opening played twice,
):
    # Each game is returned as they finish, so no need to wait for
    # every game to finish before you can start processing.

    # In this case just print out the game PGN.
    print(pgn, "\n")
```

Which could output something like this:

``` text
[Event "?"]
[Site "?"]
[Date "2019-02-07"]
[Round "1"]
[White "Stockfish 10 64 POPCNT"]
[Black "Goldfish v1.8.1"]
[Result "1-0"]
[FEN "rn3rk1/pp2ppbp/2pp1np1/q7/2PPP3/2N2BPP/PP3P2/R1BQ1RK1 b - - 0 10"]
[SetUp "1"]

10... Nbd7 11. Qe2 g5 12. Rd1 Kh8 13. Bd2 Rg8 14. b4 Qxb4 15. Nd5 Qb2 16. Nxe7 Rge8 17. Nf5 Re6 18. Qd3 h6 19. Rdb1 Ne5 20. dxe5 Qxe5 21. Bc3 Qc5 22. Rxb7 Rf8 23. Rab1 a6 24. Bg4 Re5 25. Bb4 Rxf5 26. Bxf5 Qe5 27. Bxd6 Rd8 28. Bxe5 Rxd3 29. Rxf7 Kg8 30. Bg6 Rd8 31. Bxf6 Bxf6 32. Rxf6 Rd2 33. Rxc6 Rxa2 34. Rb8+ Kg7 35. Rb7+ Kg8 36. Rc8# 1-0

[Event "?"]
[Site "?"]
[Date "2019-02-07"]
[Round "1"]
[White "Goldfish v1.8.1"]
[Black "Stockfish 10 64 POPCNT"]
[Result "0-1"]
[FEN "rn3rk1/pp2ppbp/2pp1np1/q7/2PPP3/2N2BPP/PP3P2/R1BQ1RK1 b - - 0 10"]
[SetUp "1"]

10... Qc7 11. Bf4 e5 12. dxe5 dxe5 13. Be3 Rd8 14. Qa4 Nbd7 15. c5 Nf8 16. Be2 Ne6 17. Rfd1 Nd4 18. Bxd4 exd4 19. Rxd4 Nd7 20. Rc4 Ne5 21. Rb4 a5 22. Rb3 Rd4 23. Nb5 cxb5 24. Qxb5 Rxe4 25. f3 Nd3 26. Bxd3 Qxg3+ 27. Kh1 Qxh3+ 28. Kg1 Bd4# 0-1

[Event "?"]
[Site "?"]
[Date "2019-02-07"]
[Round "2"]
[White "Stockfish 10 64 POPCNT"]
[Black "Goldfish v1.8.1"]
[Result "1-0"]
[FEN "r1bq1rk1/2pnbppp/p2p1n2/1p2p3/3PP3/1BP2N1P/PP3PP1/RNBQR1K1 w - - 1 11"]
[SetUp "1"]

11. c4 Bb7 12. Nc3 b4 13. Nd5 Nxd5 14. exd5 Bf6 15. Ba4 exd4 16. Nxd4 Nb6 17. Bc2 g6 18. b3 Qd7 19. a3 bxa3 20. Rxa3 Rfe8 21. Be3 a5 22. Bd3 h6 23. Nf3 Bc3 24. Re2 Kh7 25. Nd4 f5 26. Ne6 c5 27. Qc2 Bh8 28. Bxf5 gxf5 29. Qxf5+ Kg8 30. Qg4+ Bg7 31. Bxh6 Re7 32. Bxg7 Rxg7 33. Qxg7+ Qxg7 34. Nxg7 Bxd5 35. Nf5 Bb7 36. Nxd6 Bc6 37. Re7 Rd8 38. Re6 Bd7 39. Rf6 Kg7 40. Ne4 Re8 41. Rxb6 Rxe4 42. Rxa5 Re5 43. Ra7 Re7 44. Rbb7 Re1+ 45. Kh2 Kh8 46. Rxd7 Re8 47. Rdc7 Kg8 48. Rxc5 Rf8 49. Kg1 Rf7 50. Rxf7 Kxf7 51. Kh2 Kf8 52. Rd5 Kg8 53. b4 Kg7 54. b5 Kf7 55. b6 Ke6 56. b7 Kf6 57. b8=Q Kf7 58. Rd6 Kg7 59. Qc7+ Kh8 60. Rd8# 1-0

[Event "?"]
[Site "?"]
[Date "2019-02-07"]
[Round "2"]
[White "Goldfish v1.8.1"]
[Black "Stockfish 10 64 POPCNT"]
[Result "0-1"]
[FEN "r1bq1rk1/2pnbppp/p2p1n2/1p2p3/3PP3/1BP2N1P/PP3PP1/RNBQR1K1 w - - 1 11"]
[SetUp "1"]

11. Na3 Bb7 12. Qd3 c5 13. dxe5 c4 14. Nxc4 bxc4 15. Qxc4 dxe5 16. Qd3 Qc7 17. Bg5 Nc5 18. Qc4 a5 19. Bc2 Ba6 20. Bxf6 Bxc4 21. Bxe5 Qb7 22. b3 Bd3 23. Bxd3 Nxd3 24. Re2 Nxe5 25. Nxe5 a4 26. Rb1 axb3 27. Rxb3 Qc7 28. Nf3 Qc4 29. Nd4 Bc5 30. Rd2 Bxd4 31. Rxd4 Qe2 32. a4 Qe1+ 33. Kh2 Qxf2 34. Rc4 Qf4+ 35. Kg1 Rac8 36. Rbb4 Qc1+ 37. Kf2 Rxc4 38. Rxc4 Qd2+ 39. Kg1 Qd3 40. Rd4 Qxc3 41. Rd1 Qe3+ 42. Kh1 Qxe4 43. a5 Qe5 44. a6 Qa5 45. a7 Qxa7 46. Rd5 Qe3 47. Kh2 Qf4+ 48. Kh1 Qf1+ 49. Kh2 Rc8 50. Rd4 Qe1 51. Rd5 Qe2 52. Ra5 Qe4 53. g4 Qf4+ 54. Kh1 Rc1+ 55. Kg2 Rc2+ 56. Kg1 Qc1# 0-1
```

The result is `4` games are played. This is because the tournament will run
`n_games * len(list(permutations(players, 2)))!` games, i.e. each player plays
every other player both as white and as black, and all of this is repeated `n_games`
times.

## Installation

`chester` is easily installed using `pip` on any platform. Note that the only officially supported Python version is `3.7` or greater.

```bash
> pip install chester
```

## Alternatives

The most known alternative to `chester` is
[cutechess](https://github.com/cutechess/cutechess). This is a much more feature rich project, aiming to do much more than `chester`.
This can be a benfit for those that need it, but it is also its downside compared to `chester`. Due to its complexity, cutechess is much less portable, and can be quite cumbersome to set up on certain systems.

`chester` on the other hand aims to be much more portable, and depends only on a compatible
version of Python and the package `python-chess` which automatically installs when you install using `pip`.


It does _not_, however, have any intentions of becoming
_feature complete_ and supporting everything imaginable. Its aim is to provide
a simple way to do a simple task - testing chess engines against each other.

