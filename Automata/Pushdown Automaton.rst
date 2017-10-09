PushdownAutomaton<T>
====================

.. highlight:: csharp


A simple `Pushdown Automaton <https://en.wikipedia.org/wiki/Pushdown_automaton>`_ with states as objects.

If your state classes have an empty constructor, the state machine can register the states automatically when needed (using *PushStateAuto<TState>()* and *ChangeStateAuto<TState>()*).
If not you should register the states yourself using *RegisterStateType<TState>()* or *RegisterStateType()* and use the regular.

The machine will automatically pool the states so you don't have to worry about it.

Whether or popping the last state is valid is up to your design.

.. todo::
    Write about how to use PushdownAutomaton.

Examples
--------
Game State Management
~~~~~~~~~~~~~~~~~~~~~
::

    public class MainScreen : PdaState<ScreenManager>
    {
        public override Begin()
        {
            // show main menu
        }

        public override Reason()
        {
            if (playButton.WasPressed)
            {
                Machine.PushStateAuto<GameScreen>();
                Machine.PushStateAuto<LoadingScreen>();
            }
            else if (howToButton.WasPressed)
                Machine.PushStateAuto<HowToPlayPopup>();
            else if (optionsButton.WasPressed)
                Machine.PushStateAuto<OptionsScreen>();
        }

        public override End()
        {
            // hide main menu
        }
    }

    public class HowToPlayPopup : PdaState<ScreenManager>
    {
        public override Begin()
        {
            // show popup containing tutorial
        }

        public override Reason()
        {
            if (closeButton.WasPressed)
                Machine.PopState();
        }

        public override End()
        {
            // hide popup containing tutorial
        }
    }

    public class OptionsScreen : PdaState<ScreenManager>
    {
        public override Begin()
        {
            // show options menu
        }

        public override Reason()
        {
            if (closeButton.WasPressed)
                Machine.PopState();
        }

        public override End()
        {
            // hide options menu
        }
    }

    public class LoadingScreen : PdaState<ScreenManager>
    {
        public override Begin()
        {
            // show loading overlay
        }

        public override Reason()
        {
            if (doneLoading)
                Machine.PopState();
        }

        public override End()
        {
            // hide loading overlay
        }
    }

    public class GameScreen : PdaState<ScreenManager>
    {
        public override Begin()
        {
            // show gameplay screen
        }

        public override Reason()
        {
            if (player.IsDead)
                Machine.ChangeStateAuto<GameOverScreen>();
        }

        public override End()
        {
            // hide gameplay screen
        }
    }

    public class ScreenManager
    {
        private PushdownAutomaton<Menu> _machine;

        public ScreenManager()
        {
            _machine = new PushdownAutomaton<Menu>(this);
            _machine.ChangeStateAuto<MainScreen>();
            _machine.PushStateAuto<LoadingScreen>();
        }

        public void Update(float deltaTime)
        {
            _machine.Update(deltaTime);
        }
    }