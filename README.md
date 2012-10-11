UnitTestSnippet
===============

A set of VisualStudio code snippet for some unit test case


Testing A PRISM EventAggregator Event
=====================================

This is the first snippet. 
Consider this scenario: we are in an MVVM WPF/WP7 application and we are using the PRISM library. In particular we are using EventAggregator as our message broker. We want to test the code of a method which subscribe an event. Someting like this :

<pre lang="csharp">
public class ViewModel
{
  private bool _isEnabled;
 
  public bool IsEnabled
  {
     get { return _isEnabled; }
  }
 
  public ViewModel(IEventAggregator eventAggregator)
  {
     eventAggregator.GetEvent<EnableFlagEvent>().Subscribe(OnEnableFlagEvent, ThreadOption.PublisherThread, true);
  }
 
  private void OnEnableFlagEvent(EventArgs obj)
  {
     _isEnabled = true;
  }
}
</pre>
In this case we want to test OnEnableFlagEvent(). The solution proposed is to mock the method subscribe of the IEventAggregator interface and to use the callback function of Moq as shown in this example:
<pre lang="csharp">
[Fact]
public void IsEnabled_ShouldBe_True_When_EnableFlagEvent_IsPublished ()
{
   Mock<IEventAggregator> eventAggregator= new Mock<IEventAggregator>();
 
   Mock<EnableFlagEvent> mockedEvent= new Mock<EnableFlagEvent>();
 
   Action<EventArgs> action = null;
 
   eventAggregator.Setup(p => p.GetEvent<EnableFlagEvent>()).Returns(mockedEvent.Object);
 
   mockedEvent.Setup(evnt => evnt.Subscribe(It.IsAny<Action<EventArgs>>(), ThreadOption.PublisherThread, true, null)).Callback<Action<EventArgs>, ThreadOption, bool, Predicate<EventArgs>>((cb, a, b, c) => action = cb);
 
   ViewModel viewModel = new ViewModel(eventAggregator.Object);
 
   action(new EventArgs());
 
   Assert.True(viewModel.IsEnabled);
}
</pre>
The [https://github.com/piccoloaiutante/UnitTestSnippet/blob/master/testEvent.snippet snippet] propose an abstraction of this case.
