#CliNodes

Install: ```pip3 install clinodes```

Example app:

```python
from clinodes.nodes import Switch, ArgNode

forced = False

class FSwitch(Switch):
    def setup(self):
        pass

    def run(self, *args):
        global forced
        forced = True


class PersonAdderNode(ArgNode):
    def setup(self):
        self.enable_any = True
        self.expects_more = True

    def run(self, *args_remained):
        for arg in args_remained:
            print("Adding person '{}' | forced: {}".format(arg, forced))


class AnimalAdderNode(ArgNode):
    def setup(self):
        self.enable_any = True
        self.expects_more = True

    def run(self, *args_remained):
        for arg in args_remained:
            print("Adding animal '{}' | forced: {}".format(arg, forced))


class AdderNode(ArgNode):
    def setup(self):
        self.commands = {
            "person": PersonAdderNode,
            "animal": AnimalAdderNode
        }
        self.switches = {
            "-f": FSwitch
        }

class RootNode(ArgNode):
    def setup(self):
        self.commands = {
            "add": AdderNode
        }

if __name__ == '__main__':
    rg = RootNode()
```

Usage:
```<program> add animal Tiger```
-> ```Adding animal 'Tiger' | forced: False```

```<program> add -f person John```
-> ```Adding person 'John' | forced: True```

