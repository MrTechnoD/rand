#!env/bin/python3

import sys, argparse, random, math, time
from consent_model import ConsentModel
from simulator import simulate

    
# script
class Script:

    def __init__(self):
        self.logs = []
    
    def log(self, event):
        print(event)
        self.logs.append(event)
    
    # save script on file
    def save(self, filename):
        with open(filename, 'w') as f:
            for line in self.logs:
                f.write(line+"\n")

    # sync
    def sync(self):
        self.log("sync")

    # advance one time step
    def step(self):
        self.log("step")
        
    # grant
    def grant(self, retro, data, data_subject, recipient, consent):
        self.log("grant "+("retro " if retro else "")+data+" "+data_subject+" "+recipient+" :"+consent)
    
    # withdraw
    def withdraw(self, retro, consent):
        self.log("withdraw "+("retro " if retro else "")+":"+consent)
    
    # collect
    def collect(self, data, data_subject, recipient):
        # self.log("assume true collect "+data+" "+data_subject+" "+recipient)
        self.log("collect "+data+" "+data_subject+" "+recipient)
    
    # access
    def access(self, data, data_subject, recipient, on_collect_from=False, on_collect_to=False):
        time = ((on_collect_from+" "+on_collect_to if on_collect_to else on_collect_from) if on_collect_from else "")
        # self.log("assume true access "+data+" "+data_subject+" "+recipient+" "+time)
        self.log("access "+data+" "+data_subject+" "+recipient+" "+time)
    
    # assume
    def assume(self, expected, action, data, data_subject, recipient, collect_from=False, collect_to=False):
        time = ((collect_from+" "+collect_to if collect_to else collect_from) if collect_from else "")
        self.log("assume "+expected+" "+action+" "+data+" "+data_subject+" "+recipient+" "+time)
    
    # create new data
    def new_data(self, data, parent="Data"):
        self.log("new data "+data+" "+parent)
    
    # create new recipient
    def new_recipient(self, recipient):
        self.log("new recipient "+recipient)
    
    # create new disjoint
    def new_disjoint(self, data1, data2):
        self.log("new disjoint "+data1+" "+data2)
    
    # create new eqiv
    def new_equiv(self, data1, data2):
        self.log("new eqiv "+data1+" "+data2)





def steps(steps_tot):

    script = Script()

    step = 1
    while step < steps_tot:
        
        # step
        step += 1
        script.step()

    # script.save("scripts\scenario.generated.steps")
    return script

def nested_data(steps_tot):

    script = Script()

    step = 1
    while step < steps_tot:

        script.new_data("Data"+str(step), ("Data"+str(step-1) if step>1 else "Data") )
        
        # step
        step += 1
        script.step()

    # script.save("scripts\scenario.generated.nested_data")
    return script

def data_and_recipient(steps_tot):

    script = Script()

    step = 1
    while step < steps_tot:

        script.new_data("Data"+str(step), ("Data"+str(step-1) if step>1 else "Data") )
        script.new_recipient("r"+str(step))
        
        # step
        step += 1
        script.step()

    # script.save("scripts\scenario.generated.nested_data")
    return script


def access(steps_tot):

    script = Script()

    script.new_data("Data0")
    script.grant(retro="true",data="Data0",data_subject="ds1",recipient="r1",consent="c0")

    step = 1
    while step < steps_tot:
        
        if step>2:
            script.assume(
                expected="true",
                action="access",
                data="Data0",
                data_subject="ds1",
                recipient="r1",
                collect_from="t"+str(step-2),
                collect_to="t"+str(step-1)
            )
        
        # step
        step += 1
        script.step()

    return script

def data_and_access(steps_tot):

    script = Script()
    
    script.new_data("Data0")
    script.grant(retro="true",data="Data0",data_subject="ds1",recipient="r1",consent="c0")

    step = 1
    while step < steps_tot:

        script.new_data("Data"+str(step), "Data"+str(step-1) )
        
        if step>2:
            script.assume(
                expected="true",
                action="access",
                data="Data"+str(step-2),
                data_subject="ds1",
                recipient="r1",
                collect_from="t"+str(step-2),
                collect_to="t"+str(step-1)
            )
        
        # step
        step += 1
        script.step()

    return script


def collect(steps_tot):

    script = Script()

    script.new_data("Data0")
    script.grant(retro="true",data="Data0",data_subject="ds1",recipient="r1",consent="c0")

    step = 1
    while step < steps_tot:

        script.assume(expected="true", action="collect", data="Data0", data_subject="ds1", recipient="r1")
        
        # step
        step += 1
        script.step()

    # script.save("scripts\scenario.generated.nested_data_collect")
    return script

def data_and_collect(steps_tot):

    script = Script()

    # root Data
    # script.new_data("data0")

    step = 1
    while step < steps_tot:

        script.new_data("Data"+str(step), ("Data"+str(step-1) if step>1 else "Data") )
        script.assume(expected="false", action="collect", data="Data"+str(step), data_subject="ds1", recipient="r1")
        
        # step
        step += 1
        script.step()

    # script.save("scripts\scenario.generated.nested_data_collect")
    return script


def disjoint_data(steps_tot):

    script = Script()

    step = 1
    while step < steps_tot:

        script.new_data("Data"+str(step), "Data")

        if step>1:
            script.new_disjoint("Data"+str(step), "Data"+str(step-1))
        
        # step
        step += 1
        script.step()

    # script.save("scripts\scenario.generated.nested_data_collect")
    return script

def disjoint_data_and_sync(steps_tot):

    script = Script()

    step = 1
    while step < steps_tot:

        script.new_data("Data"+str(step), "Data")

        if step>1:
            script.new_disjoint("Data"+str(step), "Data"+str(step-1))
        
        script.sync()

        # step
        step += 1
        script.step()

    # script.save("scripts\scenario.generated.nested_data_collect")
    return script


    script = Script()

    step = 1
    while step < 1000:

        script.new_data("Data"+str(step), "Data")

        if step>1:
            script.new_disjoint("Data"+str(step), "Data"+str(step-1))
        
        script.sync()

        # step
        step += 1
        script.step()

    # script.save("scripts\scenario.generated.nested_data_collect")
    return script

def disjoint_data_and_collection(steps_tot):

    script = Script()

    script.new_recipient("r1")

    step = 1
    while step < steps_tot:

        script.new_data("Data"+str(step), "Data")

        if step>1:
            script.new_disjoint("Data"+str(step), "Data"+str(step-1))
        
        script.collect(data="Data"+str(step), data_subject="ds1", recipient="r1")

        # step
        step += 1
        script.step()

    return script

def disjoint_data_and_collect(steps_tot):

    script = Script()

    script.new_recipient("r1")

    step = 1
    while step < steps_tot:

        script.new_data("Data"+str(step), "Data")

        if step>1:
            script.new_disjoint("Data"+str(step), "Data"+str(step-1))
        
        script.assume(expected="false", action="collect", data="Data"+str(step), data_subject="ds1", recipient="r1")

        # step
        step += 1
        script.step()

    # script.save("scripts\scenario.generated.nested_data_collect")
    return script


def realistic(steps_tot):

    script = Script()

    script.new_recipient("r0")
    latest_recipient = "r0"

    script.new_data("Data0", "Data")
    latest_data = "Data0"

    step = 1
    while step < steps_tot:
        
        # Every week software is released
        if step%7==0:
            # new nested data
            script.new_data("Data"+str(step), latest_data)
            latest_data = "Data"+str(step)
            # new recipient
            script.new_recipient("r"+str(step))
            latest_recipient = "r"+str(step)
        
        # Every 90 days policy is consented
        if step%90==0:
            # grant
            script.grant(retro=False,data=latest_data,data_subject="ds1",recipient=latest_recipient,consent="c"+str(step))
            # withdraw
            if (step>90):
                script.withdraw(retro=True,consent="c"+str(step-90))
                
        # Every day
        if step>2:
            script.assume(expected="true", action="collect", data=latest_data, data_subject="ds1", recipient=latest_recipient)
            # script.collect(data=latest_data, data_subject="ds1", recipient=latest_recipient)
            script.assume(expected="true",action="access", data=latest_data, data_subject="ds1", recipient=latest_recipient,  collect_from="t"+str(step-2), collect_to="t"+str(step-1))
            # script.access(data=latest_data, data_subject="ds1", recipient=latest_recipient)

        # step
        step += 1
        script.step()

    # script.save("scripts\scenario.generated.nested_data_collect")
    return script



def generate(generator_fn_name, steps_tot=500):
    
    file_name = "scripts\\gen."+generator_fn_name+str(round(time.time()))
    
    # generate the script
    script = globals()[generator_fn_name](steps_tot)
    script.save(file_name)
    print("Done script generation of: "+file_name)
    
    # run simulation
    print("Starting simulation of "+file_name)
    model = ConsentModel.load("models\init.owl")
    simulate(file_name, model)


def main(argv):
    parser = argparse.ArgumentParser(
        description='Generate consent model scripts')
    parser.add_argument('generator_fn', type=str,
                        help='the function containing the loop to generate the script')
    parser.add_argument('steps_tot', type=int,
                        help='total number of steps to simulate')
    args = parser.parse_args()
    
    generate(args.generator_fn, args.steps_tot)



if __name__ == "__main__":
    main(sys.argv[1:])
