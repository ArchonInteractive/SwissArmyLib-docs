FiniteStateMachine<T>
=====================

.. highlight:: csharp

A simple `Finite State Machine <https://en.wikipedia.org/wiki/Finite-state_machine>`_ with states as objects inspired by Prime31's excellent `StateKit <https://github.com/prime31/StateKit>`_.

If your state classes have an empty constructor, the state machine can create the states automatically when needed (using *ChangeStateAuto<T>()*). If not you should create the state instance yourself and register the state in the machine.

Whether or not a null state is valid is up to your design. 

.. todo::
    Write how to use FiniteStateMachine<T>

Examples
--------
Simple machine without automatic state creation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Let's create some oversimplified AI for an enemy that should chase the player down and attack him once he's within range.

::

    public class IdleState : FsmState<Enemy>
    {
        public override void Begin()
        {
            base.Begin();

            // start idle animation
        }

        public override void Reason()
        {
            base.Reason();

            if (Context.Target != null)
            {
                // we've gained a target, let's chase him down!
                Machine.ChangeState<ChaseState>();
            }
        }
    }

    public class ChaseState : FsmState<Enemy>
    {
        public override void Begin()
        {
            base.Begin();

            // start running animation
        }

        public override void Reason()
        {
            base.Reason();

            if (Context.Target == null)
            {
                // we lost our target, return to idle state
                Machine.ChangeState<IdleState>();
            }
            else if (Context.GetDistanceToTarget() < Context.AttackRange)
            {
                // we're within range to attack!
                Machine.ChangeState<AttackState>();
            }
        }

        public override void Act(float deltaTime)
        {
            base.Act(deltaTime);

            // move towards the target player
            Context.Move(dirToTarget);
        }
    }

    public class AttackState : FsmState<Enemy>
    {
        public override void Begin()
        {
            base.Begin();

            // start attack animation
        }

        public override void Reason()
        {
            base.Reason();

            if (Context.Target == null)
            {
                // target is lost, return to normal
                Machine.ChangeState<IdleState>();
            }
            else if (Context.GetDistanceToTarget() > Context.AttackRange)
            {
                // the player is getting away! return to chasing him
                Machine.ChangeState<ChaseState>();
            }
        }

        public override void Act(float deltaTime)
        {
            base.Act(deltaTime);

            // damage target
        }
    }

    public class Enemy : MonoBehaviour
    {
        public Player Target { get; set; }

        FiniteStateMachine<Enemy> _stateMachine;

        void Awake()
        {
            _stateMachine = new FiniteStateMachine<Enemy>(this, new IdleState());
            _stateMachine.RegisterState(new ChaseState());
            _stateMachine.RegisterState(new AttackState());
        }

        void Update()
        {
            // look for the player and set the Target property

            // update the active state
            _stateMachine.Update(Time.deltaTime);
        }

        float GetDistanceToTarget()
        {
            // return distance
        }
    }

Simple machine with automatic state creation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Same code as before, but with two small differences:

* We don't register the states after creating the machine
* We use *ChangeStateAuto<T>()*

::

    public class IdleState : FsmState<Enemy>
    {
        public override void Begin()
        {
            base.Begin();

            // start idle animation
        }

        public override void Reason()
        {
            base.Reason();

            if (Context.Target != null)
            {
                // we've gained a target, let's chase him down!
                Machine.ChangeStateAuto<ChaseState>();
            }
        }
    }

    public class ChaseState : FsmState<Enemy>
    {
        public override void Begin()
        {
            base.Begin();

            // start running animation
        }

        public override void Reason()
        {
            base.Reason();

            if (Context.Target == null)
            {
                // we lost our target, return to idle state
                Machine.ChangeStateAuto<IdleState>();
            }
            else if (Context.GetDistanceToTarget() < Context.AttackRange)
            {
                // we're within range to attack!
                Machine.ChangeStateAuto<AttackState>();
            }
        }

        public override void Act(float deltaTime)
        {
            base.Act(deltaTime);

            // move towards the target player
            Context.Move(dirToTarget);
        }
    }

    public class AttackState : FsmState<Enemy>
    {
        public override void Begin()
        {
            base.Begin();

            // start attack animation
        }

        public override void Reason()
        {
            base.Reason();

            if (Context.Target == null)
            {
                // target is lost, return to normal
                Machine.ChangeStateAuto<IdleState>();
            }
            else if (Context.GetDistanceToTarget() > Context.AttackRange)
            {
                // the player is getting away! return to chasing him
                Machine.ChangeStateAuto<ChaseState>();
            }
        }

        public override void Act(float deltaTime)
        {
            base.Act(deltaTime);

            // damage target
        }
    }

    public class Enemy : MonoBehaviour
    {
        public Player Target { get; set; }

        FiniteStateMachine<Enemy> _stateMachine;

        void Awake()
        {
            _stateMachine = new FiniteStateMachine<Enemy>(this, new IdleState());
        }

        void Update()
        {
            // look for the player and set the Target property

            // update the active state
            _stateMachine.Update(Time.deltaTime);
        }

        float GetDistanceToTarget()
        {
            // return distance
        }
    }